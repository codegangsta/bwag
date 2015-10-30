# Tips and Tricks

## Wrap a http.HandlerFunc closure
Sometimes you want to pass data to a http.HandlerFunc on initialization. This
can easily be done by creating a closure of the `http.HandlerFunc`:

``` go
func MyHandler(database *sql.DB) http.Handler {
  return http.HandlerFunc(func(rw http.ResponseWriter, r *http.Request) {
    // you now have access to the *sql.DB here
  })
}
```

## Using `gorilla/context` for request-specific data
It is pretty often that we need to store and retrieve data that is specific to
the current HTTP request. Use `gorilla/context` to map values and retrieve them
later. It contains a global mutex on a map of request objects.

``` go
func MyHandler(w http.ResponseWriter, r *http.Request) {
    val := context.Get(r, "myKey")

    // returns ("bar", true)
    val, ok := context.GetOk(r, "myKey")
    // ...

}
```
