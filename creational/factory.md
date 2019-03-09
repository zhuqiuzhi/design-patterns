# 工厂方法模式

工厂方法设计模式允许在创建对象时，不需要指定对象的特定类型。

## 解决的问题

一个框架在需要为一系列应用标准化架构模型，但又要允许每个应用能定义它们领域专用的对象和提供对象的实例。

## 实现

这个例子的实现展示了通过不同的后端来提供文件系统，例如 内存文件系统，磁盘文件系统

### 接口

```go
package http

import (
	"io"
	"os"
)

// A FileSystem implements access to a collection of named files.
// The elements in a file path are separated by slash ('/', U+002F)
// characters, regardless of host operating system convention.
type FileSystem interface {
	Open(name string) (File, error)
}

// A File is returned by a FileSystem's Open method and can be
// served by the FileServer implementation.
//
// The methods should behave the same as those on an *os.File.
type File interface {
	io.Closer
	io.Reader
	io.Seeker
	Readdir(count int) ([]os.FileInfo, error)
	Stat() (os.FileInfo, error)
}
```

### 实现同一接口的不同实现


```go
package data

import (
	"net/http"
)

type StorageType int

const (
    DiskStorage StorageType = 1 << iota
    TempStorage
    MemoryStorage
)

type MemoryFileSystem map[string][]byte

func (m MemoryFileSystem) Open(name string) (http.File, error) {
    //... 具体实现可以参考 https://github.com/lunny/tango/blob/master/static_test.go 
}

// A Dir implements FileSystem using the native file system restricted to a
// specific directory tree.
//
// While the FileSystem.Open method takes '/'-separated paths, a Dir's string
// value is a filename on the native file system, not a URL, so it is separated
// by filepath.Separator, which isn't necessarily '/'.
//
// An empty Dir is treated as ".".
type Dir string

func (d Dir) Open(name string) (http.File, error) {
    //... 具体实现可以看 https://github.com/golang/go/blob/master/src/net/http/fs.go
}

func NewFilesSystem(t StorageType) http.FileSystem{
    switch t {
    case MemoryStorage:
        return newMemoryStorage( /*...*/ )
    case DiskStorage:
        return newDiskStorage( /*...*/ )
    default:
        return newTempStorage( /*...*/ )
    }
}
```

## 使用方法

通过 工厂方法（data.NewStore）, 使用者可以指定他们想要的存储系统

```go
s, _ := data.NewStore(data.MemoryStorage)
f, _ := s.Open("file")

n, _ := f.Write([]byte("data"))
defer f.Close()
```

## go 标准库里的例子

Open opens a database specified by its database driver name and a
driver-specific data source name

```go
func Open(driverName, dataSourceName string) (*DB, error) {
	driversMu.RLock()
	driveri, ok := drivers[driverName]
	driversMu.RUnlock()
	if !ok {
		return nil, fmt.Errorf("sql: unknown driver %q (forgotten import?)", driverName)
	}

	if driverCtx, ok := driveri.(driver.DriverContext); ok {
		connector, err := driverCtx.OpenConnector(dataSourceName)
		if err != nil {
			return nil, err
		}
		return OpenDB(connector), nil
	}

	return OpenDB(dsnConnector{dsn: dataSourceName, driver: driveri}), nil
```
