# Introduction

Welcome to **Building Web Apps with Go**! If you are reading this then you have just started your journey from noob to pro. No seriously, web programming in Go is so fun and easy that you won't even notice how much information you are learning along the way!

Before we get into all the nitty gritty details, let's start with some ground rules:

## Prerequisites
To keep this tutorial small and focused, I'm assuming that you are prepared in the following ways:

1. You have installed the [Go Programming Language](https://golang.org).
2. You have setup a `GOPATH` by following the [How to Write Go Code](https://golang.org/doc/code.html#Organization) tutorial.
3. You are somewhat familiar with the basics of Go. (The [Go Tour](http://tour.golang.org) is a pretty good place to start)
4. You have installed all the [required packages](#required-packages)
5. You have installed the [Heroku Toolbelt](https://toolbelt.heroku.com/)
6. You have a [Heroku](https://id.heroku.com/signup) account

## Required Packages
For the most part we will be using the built in packages from the standard library to build out our web apps. Certain lessons such as Databases, Middleware, URL Routing, and Controllers will require a third party package. Here is a list of all the go packages you will need to install before starting:

| Name | Import Path |
| -- | -- |
| [Gorilla Mux](https://github.com/gorilla/mux) | github.com/gorilla/mux |
| [Negroni](https://github.com/codegangsta/negroni) | github.com/codegangsta/negroni |
| [Controller](https://github.com/codegangsta/controller) | github.com/codegangsta/controller |
| [Black Friday](https://github.com/russross/blackfriday) | github.com/russross/blackfriday |
| [Render](https://github.com/unrolled/render/tree/v1) | gopkg.in/unrolled/render.v1 |
| [SQLite3](https://github.com/mattn/go-sqlite3) | github.com/mattn/go-sqlite3 |

You can install (or update) these packages by running the following command in your console

``` bash
go get -u <import_path>
```

For instance, if you wish to install Negroni, the following command would be:

``` bash
go get -u github.com/codegangsta/negroni
```
