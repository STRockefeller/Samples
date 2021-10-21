# Defer

Reference:

[ITHELP](https://ithelp.ithome.com.tw/articles/10242498)



很精美的blog，用動圖來解釋defer的運作，內容相對基礎，以後如果忘掉怎麼用可以拿來快速複習

https://blog.learngoprogramming.com/golang-defer-simplified-77d3b2b817ff

跟上面同一個作者寫的，探討defer使用上的各種問題，比較深入一點，目前一共三篇

https://blog.learngoprogramming.com/gotchas-of-defer-in-go-1-8d070894cb01

## Abstract

Defer 是一個比較小的主題，其實沒甚麼可以寫的，但它又是`Go`的特色之一(至少我之前都沒有接處過類似的概念)，所以還是決定獨立為他寫一篇筆記。

Defer 顧名思義就是能拖就拖，最 後關頭才做事的意思。



## How to Use

把想要最後做的事情，前面加入`defer`關鍵字就好了

```go
func main() {
	defer fmt.Println("拖到最後才做")
	fmt.Println("數到10")
	for i := 1; i < 11; i++ {
		fmt.Println(i)
	}
}
```

```
數到10
1
2
3
4
5
6
7
8
9
10
拖到最後才做
```



可以看出這東西並不是非常必要的，它的用處更多的體現在**備忘**上，把一定要做但又不是現在馬上要做的事情先寫出來的感覺，例如以下情境:

* 開檔後記得關檔

* 開啟DB後記得關閉

* Lock後記得Unlock

雖然這些情況如果在`C#`都可以用一個大括弧`{}`框起來完事...





**注意**:若程式提前結束(例如`os.Exit(0)`)，`defer`的動作很可能不會被執行到。



## Multi Defer

逆解連鎖的概念吧，後發先至

```go
func main() {
	for i := 1; i < 11; i++ {
		defer fmt.Println(i)
	}
}
```

```
10
9
8
7
6
5
4
3
2
1
```



## Value Change

看看下面的例子吧

```go
func main() {
	var a int
	defer fmt.Println(a) //0
	a = 10
}
```

```go
func main() {
	var a int
	a = 10
	defer fmt.Println(a) //10
	a = 20
}
```

```go
func main() {
	var a int
	defer repeat(a)
	a = 10
}

func repeat(inp int) {
	println("get ", inp) //get  0
}
```

如果有傳入(值型別的)變數的話，`defer`會先把當下的變數值存起來，等到執行時再用當時的變數執行，而不會重新獲取當下的變數值。

參考型別或傳址則會獲取當前數值

```go
package main
import "fmt"
func main() {
	var a int
	defer func(){
		fmt.Println(a) //10
	}()
	a = 10
}
```

```go
package main
import "fmt"
func main() {
	var a int
	defer func(b int){
		fmt.Println(b) //0
	}(a)
	a = 10
}
```



## About Defer Close



使用defer來關閉檔案或資料庫算是比較常見的用法，不過有一些細節需要特別留意



**defer無法接收回傳值**

參考[Don't defer Close() on writable files](https://www.joeshaw.org/dont-defer-close-on-writable-files/)

主要提到因為defer無法獲取回傳值(通常是error)，可能導致非預期的問題發生

如果在意Close是否出錯並且希望準備對應的處理的話，避免使用defer close或許是比較好的做法



**Error Handle要在defer之前**

例如這是不好的做法

```go
	file, err := os.Create("../../mock/mock_func.go")
	defer file.Close()
	if err != nil {
		return err
	}
```

這是比較好的做法

```go
	file, err := os.Create("../../mock/mock_func.go")
	if err != nil {
		return err
	}
	defer file.Close()
```



至於原因，就必須插播一下defer的另一個特性，眾所皆知defer會在return之前被執行，但實際上是"在寫在defer之後的return之前被執行"

以上面的例子來說，假如說最開始開黨或建立檔案失敗，`file`物件很可能會是nil，第一種情況會執行到`file.Close`但第二種則不會