# JSON

JSON is quickly becoming the ubiquitous serialization format for web APIs, so I
figure JSON would be the most relevant when learning how to build web apps
using Go. Fortunately, JSON is also very easy to work with in Go. It is
extremely easy to turn existing Go structs into JSON using the `encoding/json`
package from the standard library.

``` go
package main

import (
  "encoding/json"
  "net/http"
)

type Profile struct {
  Name    string
  Hobbies []string
}

func main() {
  http.HandleFunc("/", ProfileHandler)
  http.ListenAndServe(":8080", nil)
}

func ProfileHandler(w http.ResponseWriter, r *http.Request) {
  profile := Profile{"Alex", []string{"snowboarding", "programming"}}

  js, err := json.Marshal(profile)
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
2. Instead of using the `json.Marshal`, method. Try using the `json.Encoder` API.
3. Figure our how to pretty print JSON with the `encoding/json` package.
