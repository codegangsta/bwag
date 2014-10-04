# HTML Templates

``` go
package main

import (
  "html/template"
  "net/http"
  "path"
)

type Profile struct {
  Name    string
  Hobbies []string
}

func main() {
  http.HandleFunc("/", foo)
  http.ListenAndServe(":3000", nil)
}

func foo(w http.ResponseWriter, r *http.Request) {
  profile := Profile{"Alex", []string{"snowboarding", "programming"}}

  fp := path.Join("templates", "index.html")
  tmpl, err := template.ParseFiles(fp)
  if err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
    return
  }

  if err := tmpl.Execute(w, profile); err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
  }
}
```
