# Middleware

If you have some code that needs to be run for every request, regardless of
the route that it will eventually end up invoking, you need some way to stack
`http.Handlers` on top of each other and run them in sequence. This problem is
solved elegantly through middleware packages. Negroni is a popular middleware
package that makes building and stacking middleware very easy while keeping the
composable nature of the Go web ecosystem intact.

Negroni comes with some default middleware such as Logging, Error Recovery, and
Static file serving. So out of the box Negroni will provide you with a lot of
value without a lot of overhead.

The example below shows how to use a Negroni stack with the built in middleware
and how to create your own custom middleware.

``` go
package main

import (
    "log"
    "net/http"

    "github.com/codegangsta/negroni"
)

func main() {
    // Middleware stack
    n := negroni.New(
        negroni.NewRecovery(),
        negroni.HandlerFunc(MyMiddleware),
        negroni.NewLogger(),
        negroni.NewStatic(http.Dir("public")),
    )

    n.Run(":8080")
}

func MyMiddleware(rw http.ResponseWriter, r *http.Request, next http.HandlerFunc) {
    log.Println("Logging on the way there...")

    if r.URL.Query().Get("password") == "secret123" {
        next(rw, r)
    } else {
        http.Error(rw, "Not Authorized", 401)
    }

    log.Println("Logging on the way back...")
}
```

## Exercises

1. Think of some cool middleware ideas and try to implement them using Negroni.
2. Explore how Negroni can be composed with `github.com/gorilla/mux` using the `http.Handler` interface.
3. Play with creating Negroni stacks for certain groups of routes instead of the entire application.
