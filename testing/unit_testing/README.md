# Unit Testing

Unit testing allows us to test a `http.HandlerFunc` directly without
running any middleware, routers, or any other type of code that might
otherwise wrap the function.

```go
package main

import (
	"fmt"
	"net/http"
)

func HelloWorld(res http.ResponseWriter, req *http.Request) {
	fmt.Fprint(res, "Hello World")
}

func main() {
	http.HandleFunc("/", HelloWorld)
	http.ListenAndServe(":3000", nil)
}
```

This is the test file. It should be placed in the same directory as
your application and name `main_test.go`.

```go
package main

import (
	"net/http"
	"net/http/httptest"
	"testing"
)

func Test_HelloWorld(t *testing.T) {
	req, err := http.NewRequest("GET", "http://example.com/foo", nil)
	if err != nil {
		t.Fatal(err)
	}

	res := httptest.NewRecorder()
	HelloWorld(res, req)

	exp := "Hello World"
	act := res.Body.String()
	if exp != act {
		t.Fatalf("Expected %s gog %s", exp, act)
	}
}
```

## Exercises
1. Change the output of `HelloWorld` to print a parameter and then test that the parameter is rendered.
2. Create a POST request and test that the request is properly handled.

