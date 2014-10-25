# Databases
One of the most asked questions I get about web development in Go is how to
connect to a SQL database. Thankfully, Go has a fantastic SQL package in the
standard library that allows us to use a whole slew of drivers for different
SQL databases. In this example we will connect to a SQLite database, but the
syntax (minus some small SQL semantics) is the same for a MySQL or PostgreSQL
database.

``` go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"net/http"

	_ "github.com/mattn/go-sqlite3"
)

func main() {
	db := NewDB()
	log.Println("Listening on :8080")
	http.ListenAndServe(":8080", ShowBooks(db))
}

func ShowBooks(db *sql.DB) http.Handler {
	return http.HandlerFunc(func(rw http.ResponseWriter, r *http.Request) {
		var title, author string
		err := db.QueryRow("select title, author from books").Scan(&title, &author)
		if err != nil {
			panic(err)
		}

		fmt.Fprintf(rw, "The first book is '%s' by '%s'", title, author)
	})
}

func NewDB() *sql.DB {
	db, err := sql.Open("sqlite3", "example.sqlite")
	if err != nil {
		panic(err)
	}

	_, err = db.Exec("create table if not exists books(title text, author text)")
	if err != nil {
		panic(err)
	}

	return db
}
```

## Exercises
1. Make use of the `Query` function on our `sql.DB` instance to extract a collection of rows and map them to structs.
2. Add the ability to insert new records into our database by using an HTML form.
3. `go get github.com/jmoiron/sqlx` and observe the improvements made over the existing database/sql package in the standard library.
