---
layout: post
title:  "Golang Discovery"
date:   2021-04-01 00:00:00 +0800
categories: go
---

After years of Java, JavaScript (or TypeScript), and SQL I wanted to discover a new language with some different features. Reading about the heavyweights Python and Golang they were the two very appealing to me...**and I picked Go :-)**

## First Contact
First contact from [official website](https://golang.org/) was smooth and installation easy even I had to read about GOPATH, GOBIN and other GO not that important at the end variables.
As most of developer nowadays I chose to try it out in VSCode which has all the necessary tooling to code in Go: it has a nice [Go VSCode extension](https://marketplace.visualstudio.com/items?itemName=golang.go) and integration is good, simply had to wait a couple of minutes that it downloads its associated tools.

One thing which I noticed quickly is **most of the important features often requiring many 3rd parties in other languages are part of the standard Go library** e.g.: IO, time, JSON or HTTP support are all part of the core.
I cannot forget to quote one of Golang most famous features: the parallelism with its unique goroutines.

Also the tooling, **most of the necessary resources a developer needs are included in go**: the formatting, the dependencies management, the test, the coverage, the performance tests, the vendoring if necessary and many others.

Go produces compiled executable that are platform specific and every binary embeds the compiled Go runtime. Therefore your Go program can run without any dependencies on the installation. Note you can cross compile Go programs: e.g. compiliung on MacOS for Linux.

Then I started coding as...well...this is what I wanted to do right?
I could have used the [Go playground](https://play.golang.org/) if I knew about it before but I would have faced quickly some limitations not having set up my dev environment.
Also the ecosystem is important in the assessment.

I created my first module and started (for go get reference refer [here](https://golang.org/pkg/cmd/go/internal/get/)

## First Impression
Go syntax is pleasant, not verbose and get rid of many superfluous characters typing in other languages.
It is also powerful, I retrieved the playful pointers that I handled much more during my studies (learning programming in C++ at the time) than in my career but Go language is more developer friendly and safer for the application as **it includes a garbage collector**.

By all the benchmarks Go is fast and has low memory footprint.

No inheritance but easy composition, interfaces which enables mocking and good testability: you are up to speed quickly coding in Go especially that the compiler is really helpful. A lot of over complicated or bad practices are simply not allowed and Go keeps things simple for the sake of the language and your applications maintainability. As gophers often say or write: it is idiomatic: i.e. the way some code is written is specific to Go and it has to be done this way to be elegant / optimal in Go.

Below a few takeaways from my first coding and language experimentation

## Types
### Primitives Types
**Literals** refers to _number, character, or string_

`int8` to `int64`, `uint8` (`byte`) to `uint64`; most often you will use anyway `int` or `uint` types (that are not consistent across platforms)

<div class="alert alert-primary d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Info:"><use xlink:href="#info-fill"/></svg>
  <div class="blogalert">
  <strong>strconv</strong> package may help to go from one type to another.
  Most commonly used methods are <i>FormatInt/FormatUint</i> and <i>ParseInt/ParseUint</i>
  </div>
</div>


**Floating point literals** `float32` or `float64`.
<div class="alert alert-primary d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Info:"><use xlink:href="#info-fill"/></svg>
  <div class="blogalert">
In absence of specific reason pick float64.
  </div>
</div>

**Rune literals** represent characters and are surrounded by single quotes.

`bool` for booleans

`time`

```Go
const shortForm = "2006-01-02"
birthday := u.Birthday.Format(shortForm)
```

Declaration:

```Go
var s string //declaration only
var s string = "hello"//declaration and assignment at once
s := "hello" //declaration and assignment at performance, short form
```

### Composite Types
It includes **array**, **slice**, **map**

Declaration:
- array

```
var zeroValuesAray [3]int
var array = [3]int{1,2,3}
var array = [...]int{1,2,3}
```

- slice

```
var nilSlice []int
slice := make([]int, 3, 5) // make(type, len, initial capacity).
var slice = []int{1,2,3} //declaring and initialising
var slice = []int{1,3:3} //skipping some index, gives {1,0,3}
var slice = []int{} //empty slice for JSON serialisation for instance
```

- map

```
var nilMap map[string]int
map := map[string]int{}
mapInhabitants := map[string][]string {
  "England": 580000,
  "Spain": 48000000,
  "France": 60000000,
}
mapInhabitants := make(map[int][]string, 3)
```
#### More About Slices
`len` returns the , len of a nil slice is 0

`append` to add content to a slice <code>slice = append(slice, "newValue")</code>

`cap` represents the memory allocated, a slice can have for instance a capacity of 4 but a length of 3: it contains 3 elements but the slice structure has been allocated to receive up to 4, till its next extension while appending elements.
cap should obvisouly always be >= len.

`slicing`
Coming back on our previous declaration example <code>x := make([]int, 3, 5) // make(type, len, initial capacity)</code>; it will create an array {0, 0, 0} with a len of 3 initialised with zero values and an initial capacity of 5.

You can subset slices with syntax slice[startOffset:endOfset]
```Go
whole := []int{1,2,3,4,5}
left := whole[:2]   // [1,2]
right := whole[2:]  // [3,4,5]
```

<div class="alert alert-warning d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Info:"><use xlink:href="#exclamation-triangle-fill"/></svg>
  <div class="blogalert">
Sliced slices share memory and so modifying one amend the other.<br/>
  </div>
</div>
<div class="alert alert-warning d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Info:"><use xlink:href="#exclamation-triangle-fill"/></svg>
  <div class="blogalert">
Be very careful with a slice of string: all characters are not always 1 byte.
  </div>
</div>

`copy` copies as many values as it can from source to destination, limited by whichever slice is smaller, and returns the number of elements copied.
```Go
i := copy(destination, source)
```

### Zero Values
<table class="table">
	<colgroup>
		<col width="50%"/>
		<col width="50%"/>
	</colgroup>
	<thead class="thead-light">
		<tr>
			<th>Type</th>
			<th>Zero Value</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				<code>int</code> (and variations)
			</td>
			<td>0</td>
		</tr>
    <tr>
      <td>
        <code>array</code>
      </td>
      <td>zero values of the type of the array</td>
    </tr>
    <tr>
      <td>
        <code>slice</code>
      </td>
      <td>nil</td>
    </tr>
    <tr>
      <td>
        <code>string</code>
      </td>
      <td>empty string</td>
    </tr>
    <tr>
      <td>
        <code>map</code>
      </td>
      <td>nil</td>
    </tr>
	</tbody>
</table>

## Logging
https://golang.org/pkg/fmt/

## Control Structure
Iterate over strings
```Go
random := "random"
for i, r := range sample { //iterate over the runes of the string
    fmt.Println(i, r, string(r))
}
```
Iterate over maps
```Go
for k, v := range m {
    fmt.Println(k, v)
}
```

<div class="alert alert-primary d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Info:"><use xlink:href="#info-fill"/></svg>
  <div class="blogalert">    
   <strong>The comma ok Idiom</strong><br/>
    As a map returns the zerol value for the key when there is no match for the key if you want to distinguish between a miss and a zero value you need to use the comma ok idiom.<br/>
    <code>
    v, ok := m["Spain"]
    </code>
  </div>
</div>

## Go Testing
```sh
go test ./...
```
verbose
go test -timeout 30s -run ^TestFindPdcsByDivision$ bitbucket.org/kdefombelle/ver1rev-backend/http/rest

clear go test cache
```sh
go clean -testcache
```
# Database Driver
Install a database driver
e.g. for mysql with [go-sql-driver](https://github.com/Go-SQL-Driver/MySQL/)
```
go get -u github.com/go-sql-driver/mysql
```

# Go and Open API
[go-swagger](https://goswagger.io/)
_"There is no point in continuing to ask. I don't have the time to spend on a openapi 3.0 implementation"_ as per [this issue comment](https://github.com/go-swagger/go-swagger/issues/1122#issuecomment-575968499)

It is part of Open API 3.0.x; Swagger documents about [it](https://swagger.io/docs/specification/authentication/cookie-authentication/)...but does not support it as explained in this [github ticket](https://github.com/swagger-api/swagger-editor/issues/1951) (except documentation is hosted on SwaggerHub)
