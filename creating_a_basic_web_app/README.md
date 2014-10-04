# Creating a Basic Web App

// Outline the Goal: Create a basic (but useful) web app in Go.

``` go
package main

import (
    "net/http"

    "github.com/russross/blackfriday"
)

func main() {
    http.HandleFunc("/markdown", GenerateMarkdown)
    http.Handle("/", http.FileServer(http.Dir("public")))
    http.ListenAndServe(":8080", nil)
}

func GenerateMarkdown(rw http.ResponseWriter, r *http.Request) {
    markdown := blackfriday.MarkdownCommon([]byte(r.FormValue("body")))
    rw.Write(markdown)
}
```

``` html
<head>
  <link href="/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

  <div class="container">

    <div class="page-title">
      <h1>Markdown Generator</h1>
      <p class="lead">Generate your markdown with Go</p>
      <hr />
    </div>

    <form action="/markdown" method="POST">
      <div class="form-group">
        <textarea class="form-control" name="body" cols="30" rows="10"></textarea>
      </div>

      <div class="form-group">
        <input type="submit" class="btn btn-primary pull-right" />
      </div>
    </form>
  </div>

  <script src="/js/bootstrap.min.js"></script>
</body>
```
