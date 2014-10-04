# Controllers

```
package main

import (
    "net/http"

    "github.com/codegangsta/controller"
    "github.com/codegangsta/controller/view"
    "github.com/gorilla/mux"
)

type BooksController struct {
    view.ViewController
}

func (c *BooksController) Index() error {
    c.View["name"] = "Jeremy"
    c.HTML(200, "books/index")
    return nil
}

func main() {
    r := mux.NewRouter()
    r.Handle("/", controller.Action((*BooksController).Index))
    http.ListenAndServe(":8080", r)
}
```
