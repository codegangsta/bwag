# Deployment

``` go
package main

import (
    "net/http"
    "os"

    "github.com/russross/blackfriday"
)

func main() {
    port := os.Getenv("PORT")
    if port == "" {
        port = "8080"
    }

    http.HandleFunc("/markdown", GenerateMarkdown)
    http.Handle("/", http.FileServer(http.Dir("public")))
    http.ListenAndServe(":"+port, nil)
}

func GenerateMarkdown(rw http.ResponseWriter, r *http.Request) {
    markdown := blackfriday.MarkdownCommon([]byte(r.FormValue("body")))
    rw.Write(markdown)
}
```

// Procfile
```
web: deploy
```

// .godir
```
deploy
```

``` bash
git init
git add -A
git commit -m "Initial Commit"
```

``` bash
heroku create -b https://github.com/kr/heroku-buildpack-go.git
```

``` bash
git push heroku master
```
