# 单例模式

单例模式是限制在整个程序中某个类型的实例只有一个并且保证只有一个。 你会在以下场景中用到它:

* 整个程序中只想要同样的一个数据库连接来查询
* 只想开一个 Secure Shell 连接到服务器
* 限制对某些变量的访问

## 实现方式1： 非并发安全

简单的实现就是使用一个 package 级别的隐藏变量, 首先判断这个变量是否为 nil,
如果不为 nil，就创建一个新的对象，并将该对象的指针保存到该变量。

```go
package icon 

import (
	"sync"
	"image"
	"os"
	"bytes"
)
	
var icons map[string]image.Image

func loadIcons() {
    icons = map[string]image.Image{
        "spades.png":   loadIcon("spades.png"),
        "hearts.png":   loadIcon("hearts.png"),
        "diamonds.png": loadIcon("diamonds.png"),
        "clubs.png":    loadIcon("clubs.png"),
    }
}

func loadIcon(name string) image.Image {
    f , err := os.Open(name)
    if err != nil {
    	panic("can't read " + name)
    }
    img, name, err := image.Decode(f)
    if err != nil {
    	panic("cat decode " + name)
    }
    return img
}

func Icon(name string) image.Image{
	if icons != nil {
		return icons[name]
	}
	loadIcons()
	return icons[name]
}
```

下面实现方法的缺陷是不是并发安全的, 因为多个 gorouting 执行 Icon(),
如果这些gorouting 在不同的CPU核上执行，最后返回的 instance 可能是不同的指针。
下面以两个 goroutine 并发执行 New() 来说明这个问题。

```
goroutine1 (运行在逻辑处理器1)     goroutine2(运行在逻辑处理2)

1. 获取 icons 值               1.获取 icons 值

2.判断 icons != nil
                                 2.判断 icons ！=nil
3. loadIcons() // long time
                                 3. loadIcons() //可能和 goutine1 loadIcons 同时进行
```

## 实现方式2：并发安全

第二种实现方式是使用读写锁来实现并发安全, 但实现复杂。

```go
package icon 

import (
	"sync"
	"image"
	"os"
	"bytes"
)
	
var icons map[string]image.Image

func loadIcons() {
    icons = map[string]image.Image{
        "spades.png":   loadIcon("spades.png"),
        "hearts.png":   loadIcon("hearts.png"),
        "diamonds.png": loadIcon("diamonds.png"),
        "clubs.png":    loadIcon("clubs.png"),
    }
}

func loadIcon(name string) image.Image {
    f , err := os.Open(name)
    if err != nil {
    	panic("can't read " + name)
    }
    img, name, err := image.Decode(f)
    if err != nil {
    	panic("cat decode " + name)
    }
    return img
}

var mux sync.RWMutex

func Icon(name string) image.Image{
	mux.RLock()
	if icons != nil {
		icon := icons[name]
		mux.RUnlock()
		return icon
	}
	mux.RLock()
	
	mux.Lock()
	if icons != nil {   // 注意: 这里必须再次检查, 因为有可能是另外一个 goroutine 释放写锁,此时 icons 已经不为 nil
	    loadIcons()
	}
	icon := icons[name]
	mux.Unlock()
	return icon
}
```

## 实现方式3： 并发安全

幸运的是，sync 提供了解决 one-time initialization 的方案 sync.Once

```go
package icon 

import (
	"sync"
	"image"
	"os"
	"bytes"
)
	
var icons map[string]image.Image

var loadIconsOnce sync.Once

func loadIcons() {
    icons = map[string]image.Image{
        "spades.png":   loadIcon("spades.png"),
        "hearts.png":   loadIcon("hearts.png"),
        "diamonds.png": loadIcon("diamonds.png"),
        "clubs.png":    loadIcon("clubs.png"),
    }
}

func loadIcon(name string) image.Image {
    f , err := os.Open(name)
    if err != nil {
    	panic("can't read " + name)
    }
    img, name, err := image.Decode(f)
    if err != nil {
    	panic("cat decode " + name)
    }
    return img
}

func Icons(name string) image.Image {
	loadIconsOnce.Do(loadIcons) // 保证了 loadIcons 只会被执行一次
	return icons[name]
}
```