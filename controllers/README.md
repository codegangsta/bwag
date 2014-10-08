# Controllers

Controllers are a fairly familiar topic in other web development communities.
Since most web developers rally around the mighty net/http interface, not many
controller implementations have caught on strongly. However, there is great
benefit in using a controller model. It allows for clean, well defined
abstractions above and beyond what the net/http handler interface can alone
provide.

In this example we will experiment with the controller pattern using
`github.com/codegangsta/controller` to construct a new controller instance on
every request. This allows us to avoid use of global variables, state, and
logic by moving domain-specific logic into it's respective controller
implementation.

``` go
package view

import (
	"net/http"

	"github.com/codegangsta/controller"
	"gopkg.in/unrolled/render.v1"
)

var Renderer = render.New(render.Options{})

type ViewController struct {
	controller.Base
	View     map[string]interface{}
	renderer *render.Render
}

func (c *ViewController) Init(rw http.ResponseWriter, r *http.Request) error {
	c.renderer = Renderer
	c.View = make(map[string]interface{})
	return c.Base.Init(rw, r)
}

func (c *ViewController) HTML(code int, name string, opts ...render.HTMLOptions) {
	c.renderer.HTML(c.ResponseWriter, code, name, c.View, opts...)
}
```

## Exercises
1. Extend this ViewController implementation to render JSON as well as HTML.
2. Play with more controller implementations, get creative.
3. Create more controllers that embed the ViewController struct.
