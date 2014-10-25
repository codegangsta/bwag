# JSON

JSON is quickly becoming the ubiquitous serialization format for web APIs, so
it may be the most relevant when learning how to build web apps
using Go. Fortunately, Go makes it simple to work with JSON -- it is
extremely easy to turn existing Go structs into JSON using the `encoding/json`
package from the standard library.

``` go
package main

import (
	"encoding/json"
	"net/http"
)

type Book struct {
	Title  string `json:"title"`
	Author string `json:"author"`
}

func main() {
	http.HandleFunc("/", ShowBooks)
	http.ListenAndServe(":8080", nil)
}

func ShowBooks(w http.ResponseWriter, r *http.Request) {
	book := Book{"Building Web Apps with Go", "Jeremy Saenz"}

	js, err := json.Marshal(book)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	w.Header().Set("Content-Type", "application/json")
	w.Write(js)
}
```

## Exercises
1. Read through the JSON API docs and find out how to rename and ignore fields for JSON serialization.
2. Instead of using the `json.Marshal` method, try using the `json.Encoder` API.
3. Figure our how to pretty print JSON with the `encoding/json` package.
