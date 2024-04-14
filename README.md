# FUSEDB

.########.##.....##..######..########....########..########.
.##.......##.....##.##....##.##..........##.....##.##.....##
.##.......##.....##.##.......##..........##.....##.##.....##
.######...##.....##..######..######......##.....##.########.
.##.......##.....##.......##.##..........##.....##.##.....##
.##.......##.....##.##....##.##..........##.....##.##.....##
.##........#######...######..########....########..########.

## Description

FuseDB is an embedded persistent relational database. Similar to sqlite, Fusedb is meant to be small and easy to use since it conforms to the built-in database/sql interface. Uses SQL language to interact with database through the database/sql interface. Written in pure Golang and no external dependencies (only those used in testing are imported) its easy to integrate similar to how you would with any other databases (ie. Mysql, sqlite etc.)

## Installation

pacakge can be installed with go get (go 1.20 or higher recommended)
```
go get github.com/kd993595/fusedb
```

## Features

Currently in development so only supports a subset of ANSI SQL, primarily the basic commands, no subquery currently supported
* Select
* Insert
* Update
* Delete
* Create

Some features still in development:
[ ] B+Tree indexing
[ ] ACID compliance
[ ] Transaction Manager
[ ] All Golang types

## Example Usage

```
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/kd993595/fusedb"
)

func main() {
	db, err := sql.Open("fusedb", "example")
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	//use to detect if issue with database opening as sql.open may not neccessarily open database
    err = db.Ping() 
	if err != nil {
		log.Fatal(err)
	}

	_, err = db.Query("CREATE TABLE 'MyTable10' (column1 int Primary Key, name char(10),column30 bool, column400 float)")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Create Query passed")

	_, err = db.Query("INSERT INTO 'MyTable10' (column1,name,column30,column400) VALUES ('1','somecharss', 'true','1.23')")
	if err != nil {
		log.Fatal(err)
	}

	_, err = db.Query("INSERT INTO 'MyTable10' (column1,name,column30,column400) VALUES ('2','10letters', 'false','4.69')")
	if err != nil {
		log.Fatal(err)
	}

	_, err = db.Query("INSERT INTO 'MyTable10' (column1,name,column30,column400) VALUES ('3','Kevin', 't','.567')")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Insert Queries passed")

	rows, err := db.Query("SELECT * FROM 'MyTable10'")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Select Query passed")

	for rows.Next() {
		var i int
		var n string
		var b bool
		var f float64
		err := rows.Scan(&i, &n, &b, &f)
		if err != nil {
			log.Fatal(err)
		}
		fmt.Printf("(%d) (%s) (%t) (%f)\n", i, n, b, f)
	}

}
```

## License
