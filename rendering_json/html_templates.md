# HTML Templates

Serving HTML is an important job for some web applications. Go has one of my
favorite templating languages to date. Not for its features, but for its
simplicity and out of the box security. Rendering HTML templates is almost as
easy as rendering JSON using the 'html/template' package from the standard
library.

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

## Exercises

1. Look through the docs for `text/template` and `html/template` package. Play with the templating language a bit to get a feel for its goals, strengths, and weaknesses.
2. In the example we parse the files on every request, which can be a lot of performance overhead. Experiment with parsing the files at the beginning of your program and executing them in your `http.Handler` (hint: make use of the `Copy()` method on `html.Template`).
3. Experiment with parsing and using multiple templates.
