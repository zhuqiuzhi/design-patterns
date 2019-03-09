# 策略模式

策略模式会定义一系列可以在运行时互换的对象(称为策略). 这个模式有三个部分:

* 使用一个策略的对象
* 定义策略行为的协议，在 go 语言中，就是一个接口
* 一系列执行同样协议的对象

## 使用场景

当有两个或者更多需要可互换的行为时，使用策略模式

## 例子

例如某个电影推荐 APP 使用了几个电影评分网站提供的电影评分服务，则可以定义一个接口来约定这些服务的行为。 
当这些电影评分网站的服务接口发生变化时，APP 本身的代码无需作出改变。这类似于三国里曹操(APP)挟天子(策略)以令诸侯(电影评分网站)。

```go
type MovieRatingService interface {
    Name() string
    FetchRating(movieTile string) (rating string, review []string)
}

type RottenTomatoesClient struct {
   BaseURL *url.URL
}

func (r *RottenTomatoesClient) Name() string {
    return "Rotten Tomatoes"
} 

func (r *RottenTomatoesClient) FetchRating(movieTile string) (rating string, review []string) {
    //do something...
    return
}

type IMDbClient struct {
   BaseURL *url.URL
}

func (i *IMDbClient) Name() string {
    return "IMDb"
} 

func (i *IMDbClient) FetchRating(movieTile string) (rating string, review []string) {
    //do something...
    return
}

type APP struct {
    movingRatingClient MovieRatingService
    // ...
}
```