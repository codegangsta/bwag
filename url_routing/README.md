# URL Routing

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
