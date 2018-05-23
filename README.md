## urlrouter
A prue url router form [naoina/denco](https://github.com/naoina/denco). I just modfiy it  to support  url query.
Such as:
 - "/test" only matches "/test"
 - "/test?" matches "/test" and "/test?aa=bb&cc=dd"

Sample:
```golang
package main

import (

	"fmt"
	"github.com/dwdcth/urlrouter"
)
type route struct {
	name string
}

func main() {
	router := urlrouter.New()
	router.Build([]urlrouter.Record{
		{"/", &route{"root"}},
		{"/user/:id", &route{"user"}},
		{"/user/:name/:id", &route{"username"}},
		{"/static/*filepath", &route{"static"}},
		{"/test?", &route{"test"}},
	})

	data, params, found := router.Lookup("/")
	// print `&main.route{name:"root"}, denco.Params(nil), true`.
	fmt.Printf("%#v, %#v, %#v\n", data, params, found)

	data, params, found = router.Lookup("/user/hoge")
	// print `&main.route{name:"user"}, denco.Params{denco.Param{Name:"id", Value:"hoge"}}, true`.
	fmt.Printf("%#v, %#v, %#v\n", data, params, found)

	data, params, found = router.Lookup("/user/hoge/7")
	// print `&main.route{name:"username"}, denco.Params{denco.Param{Name:"name", Value:"hoge"}, denco.Param{Name:"id", Value:"7"}}, true`.
	fmt.Printf("%#v, %#v, %#v\n", data, params, found)

	data, params, found = router.Lookup("/static/path/to/file")
	// print `&main.route{name:"static"}, denco.Params{denco.Param{Name:"filepath", Value:"path/to/file"}}, true`.
	fmt.Printf("%#v, %#v, %#v\n", data, params, found)

	// both /test and /test?params can found
	data, params, found = router.Lookup("/test")
	// print `&main.route{name:"test"}, urlrouter.Params(nil), true`.
	fmt.Printf("%#v, %#v, %#v\n", data, params, found)

	data, params, found = router.Lookup("/test?hello=world&name=tom")
	// print `&main.route{name:"test"}, urlrouter.Params{urlrouter.Param{Name:"", Value:"hello=world&name=tom"}}, true`.
	fmt.Printf("%#v, %#v, %#v\n", data, params, found)

}
```
[中文说明](https://www.cnblogs.com/xdao/p/golang_urlrouter.html)
