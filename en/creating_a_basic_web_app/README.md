# Creating a Basic Web App

Now that we are done going over the basics of HTTP, let's create a simple but
useful web application in Go.

Pulling from our fileserver program that we implemented last chapter, we will
implement a Markdown generator using the `github.com/russross/blackfriday`
package.

## HTML Form

For starters, we will need a basic HTML form for the markdown input: 

``` html
<html>
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
</html>
```

Put this HTML into a file named `index.html` in the "public" folder of our application
and the `bootstrap.min.css` from http://getbootstrap.com/ in the "public/css" folder.
Notice that the form makes an HTTP POST to the "/markdown" endpoint of our
application. We don't actually handle that route right now, so let's add it.

## The "/markdown" route

The program to handle the '/markdown' route and serve the public `index.html`
file looks like this:

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

Let's break it down into smaller pieces to get a better idea of what is going
on.

``` go
http.HandleFunc("/markdown", GenerateMarkdown)
http.Handle("/", http.FileServer(http.Dir("public")))
```

We are using the `http.HandleFunc` and `http.Handle` methods to define some
simple routing for our application. It is important to note that calling
`http.Handle` on the "/" pattern will act as a catch-all route, so we define
that route last. `http.FileServer` returns an `http.Handler` so we use
`http.Handle` to map a pattern string to a handler. The alternative method,
`http.HandleFunc`, uses an `http.HandlerFunc` instead of an `http.Handler`.
This may be more convenient, to think of handling routes via a function
instead of an object.

``` go
func GenerateMarkdown(rw http.ResponseWriter, r *http.Request) {
    markdown := blackfriday.MarkdownCommon([]byte(r.FormValue("body")))
    rw.Write(markdown)
}
```

Our GenerateMarkdown function implements the standard `http.HandlerFunc`
interface and renders HTML from a form field containing
markdown-formatted text. In this case, the content is retrieved
with `r.FormValue("body")`. It is very common to get input from the
`http.Request` object that the `http.HandlerFunc` receives as an argument.
Some other examples of input are the `r.Header`, `r.Body`, and `r.URL` members.

We finalize the request by writing it out to our `http.ResponseWriter`. Notice
that we didn't explicitly send a response code. If we write out to the response
without a code, the `net/http` package will assume that the response is a `200
OK`. This means that if something did happen to go wrong, we should set the
response code via the `rw.WriteHeader()` method.

``` go
http.ListenAndServe(":8080", nil)
```

The last bit of this program starts the server, we pass `nil` as our handler,
which assumes that the HTTP requests will be handled by the `net/http` packages
default `http.ServeMux`, which is configured using `http.Handle` and
`http.HandleFunc`, respectively.

And that is all you need to be able to generate markdown as a service in Go. It
is a surprisingly small amount of code for the amount of heavy lifting it does.
In the next chapter we will learn how to deploy this application to the web
using Heroku.
