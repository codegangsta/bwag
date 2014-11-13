# Controllers

Controllers are a fairly familiar topic in other web development communities.
Since most web developers rally around the mighty net/http interface, not many
controller implementations have caught on strongly. However, there is great
benefit in using a controller model. It allows for clean, well defined
abstractions above and beyond what the net/http handler interface can alone
provide.

## Handler Dependencies

In this example we will experiment with building our own controller
implementation using some standard features in Go. But first, lets start with
the problems we are trying to solve. Say we are using the `render` library that
we talked about in previous chapters:

``` go
var Render = render.New(render.Options{})
```

If we want our `http.Handler`s to be able access our `render.Render` instance,
we have a couple options.

**1. Use a global variable:** This isn't too bad for small programs, but when
the program gets larger it quickly becomes a maintenance nightmare.

**2. Pass the variable through a closure to the http.Handler: ** This is a
great idea, and we should be using it most of the time. The implementation ends
up looking like this:

``` go
func MyHandler(r *render.Render) http.Handler {
  return http.HandlerFunc(func(rw http.ResponseWriter, r *http.Request) {
    // now we can access r
  })
}
```

## Case for Controllers

When your program grows in size, you will start to notice that many of your
`http.Handler`s will share the same dependencies and you will have a lot of
these closurized `http.Handlers` with the same arguments. The way I like to
clean this up is to write a little base controller implementation that affords
me a few wins:

1. Allows me to share the dependencies across `http.Handler`s that have similar goals or concepts.
2. Avoids global variables and functions for easy testing/mocking.
3. Gives me a more centralized and Go-like mechanism for handling errors.

The great part about controllers is that it gives us all these things without
importing an external package! Most of this functionality comes from clever use
of the Go feature set, namely Go structs and embedding. Let's take a look at the
implementation.

``` go
package main

import "net/http"

// Action defines a standard function signature for us to use when creating
// controller actions. A controller action is basically just a method attached to
// a controller.
type Action func(rw http.ResponseWriter, r *http.Request) error

// This is our Base Controller
type AppController struct{}

// The action function helps with error handling in a controller
func (c *AppController) Action(a Action) http.Handler {
	return http.HandlerFunc(func(rw http.ResponseWriter, r *http.Request) {
		if err := a(rw, r); err != nil {
			http.Error(rw, err.Error(), 500)
		}
	})
}
```

Thats it! That is all the implementation that we need to have the power of
controllers at our fingertips. All we have left to do is implement an example
controller:

``` go
package main

import (
	"net/http"

	"gopkg.in/unrolled/render.v1"
)

type MyController struct {
	AppController
	*render.Render
}

func (c *MyController) Index(rw http.ResponseWriter, r *http.Request) error {
	c.JSON(rw, 200, map[string]string{"Hello": "JSON"})
	return nil
}

func main() {
	c := &MyController{Render: render.New(render.Options{})}
	http.ListenAndServe(":8080", c.Action(c.Index))
}
```

## Exercises
1. Extend `MyController` to have multiple actions for different routes in your application.
2. Play with more controller implementations, get creative.
3. Override the `Action` method on `MyController` to render a error HTML page.
