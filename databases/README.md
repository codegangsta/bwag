# Databases
One of the most asked questions I get about web development in Go is how to
connect to a SQL database. Thankfully Go has a fantastic SQL package in the
standard library that allows us to use a whole slew of drivers for different
SQL databases. In this example we will connect to a SQLite database, but the
syntax (minus some small SQL semantics) is the same for a MySQL or PostgreSQL
database.

``` go
package main

import (
  "database/sql"
	"log"

	_ "github.com/mattn/go-sqlite3"
)

func NewDB() *sql.DB {
	db, err := sql.Open("sqlite3", "example.sqlite")
	checkErr(err)

	_, err = db.Exec("create table if not exists posts(title text, body text)")
	checkErr(err)

	return db
}

func checkErr(err error) {
	if err != nil {
		log.Fatalln(err)
	}
}
```

## Exercises
1. Now that we have written the code to initialize the database, create some HTTP handlers to add data to the database. Use an html form to POST data to the endpoint and insert it into the sqlite database.
2. Make use of the `Query` function on our `sql.DB` instance to extract a collection of rows and map them to structs.
3. go get github.com/jmoiron/sqlx and observe the improvements made over the existing database/sql package in the standard library.
