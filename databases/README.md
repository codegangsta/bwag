# Databases

// db.go. a basic setup for intializing a database
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
