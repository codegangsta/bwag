# Deployment
Heroku makes deploying applications easy. It is a perfect platform for small to
medium size web applications that are willing to sacrifice a little bit of
flexibility in infrastructure to gain a fairly pain-free environment for
deploying and maintaining web applications.

I am choosing to deploy our web application to Heroku for the sake of this
tutorial because in my experience it has been the fastest way to get a web
application up and running in no time. Remember that the focus of this tutorial
is how to build web applications in Go and not getting caught up in all
of the distraction of provisioning, configuring, deploying, and maintaining the
machines that our Go code will be run on.

## Getting setup
If you don't already have a Heroku account, sign up at
[id.heroku.com/signup](https://id.heroku.com/signup). It's quick, easy and free.

Application management and configuration is done through the Heroku toolbelt,
which is a free command line tool maintained by Heroku. We will be using it to
create our application on Heroku. You can get it from
[toolbelt.heroku.com](https://toolbelt.heroku.com/).

## Changing the Code

To make sure the application from our last chapter will work on Heroku, we will
need to make a few changes. Heroku gives us a `PORT` environment variable
and expects our web application to bind to it. Let's start by importing the
"os" package so we can grab that `PORT` environment variable:

``` go
import (
    "net/http"
    "os"

    "github.com/russross/blackfriday"
)
```

Next, we need to grab the `PORT` environment variable, check if it is set, and
if it is we should bind to that instead of our hardcoded port (8080).

``` go
port := os.Getenv("PORT")
if port == "" {
  port = "8080"
}
```

Lastly, we want to bind to that port in our `http.ListenAndServe` call:

``` go
http.ListenAndServe(":"+port, nil)
```

The final code should look like this:

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

## Configuration

We need a couple small configuration files to tell Heroku how it should run our
application. The first one is the `Procfile`, which allows us to define which
processes should be run for our application. By default, Go will name the
executable after the containing directory of your main package. For instance,
if my web application lived in `GOPATH/github.com/codegangsta/bwag/deployment`, my
`Procfile` will look like this:

```
web: deployment
```

Specifically to run Go applications, we need to also specify a `.godir` file to
tell Heroku which dir is in fact our package directory.

```
deployment
```

## Deployment

Once all these things in place, Heroku makes it easy to deploy.


Initialize the project as a Git repository:
``` bash
git init
git add -A
git commit -m "Initial Commit"
```

Create your Heroku application (specifying the Go buildpack):
``` bash
heroku create -b https://github.com/kr/heroku-buildpack-go.git
```

Push it to Heroku and watch your application be deployed!
``` bash
git push heroku master
```

View your application in your browser:
``` bash
heroku open
```
