# URL Routing

For some simple applications, the default `http.ServeMux` can take you pretty
far. If you need more power in how you parse URL endpoints and route them to the
proper handler, you may need to pull in a third party routing framework. For
this tutorial, we will use the popular `github.com/gorilla/mux` library as our
router. `github.com/gorilla/mux` is a great choice for a router as it has an
interface that is familiar for `http.ServeMux` users, yet has a ton of extra
features built around the idea of finding the right http.Handler for the given
URL path.

In this example, we will create some routing for a RESTful resource called
"posts". Below we define mechanisms to view index, show, create, update,
destroy, and edit posts. Thankfully with `github.com/gorilla/mux`, we don't
have to do too much copy-pasting to accomplish this.

``` go
package main

import (
    "fmt"
    "net/http"

    "github.com/gorilla/mux"
)

func main() {
    r := mux.NewRouter().StrictSlash(true)
    r.HandleFunc("/", HomeHandler)

    // Posts collection
    posts := r.Path("/posts").Subrouter()
    posts.Methods("GET").HandlerFunc(PostsIndexHandler)
    posts.Methods("POST").HandlerFunc(PostsCreateHandler)

    // Posts singular
    post := r.PathPrefix("/posts/{id}/").Subrouter()
    post.Methods("GET").Path("/edit").HandlerFunc(PostEditHandler)
    post.Methods("GET").HandlerFunc(PostShowHandler)
    post.Methods("PUT", "POST").HandlerFunc(PostUpdateHandler)
    post.Methods("DELETE").HandlerFunc(PostDeleteHandler)

    fmt.Println("Starting server on :3000")
    http.ListenAndServe(":3000", r)
}

func HomeHandler(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(rw, "Home")
}

func PostsIndexHandler(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(rw, "posts index")
}

func PostsCreateHandler(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(rw, "posts create")
}

func PostShowHandler(rw http.ResponseWriter, r *http.Request) {
    id := mux.Vars(r)["id"]
    fmt.Fprintln(rw, "showing post", id)
}

func PostUpdateHandler(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(rw, "post update")
}

func PostDeleteHandler(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(rw, "post delete")
}

func PostEditHandler(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(rw, "post edit")
}
```

## Exercises

1. Explore the documentation for `github.com/gorilla/mux`.
2. Play with the different chain methods for creating filters and subrouters.
3. Find how well `github.com/gorilla/mux` plays nicely with existing `http.Handler`s like `http.FileServer`
