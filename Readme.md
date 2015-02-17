# mongod

[![Build Status](https://travis-ci.org/go-mong/mongod.svg?branch=master)](https://travis-ci.org/go-mong/mongod)
[![GoDoc](https://godoc.org/github.com/go-mong/mongod.v1?status.svg)](http://godoc.org/github.com/go-mong/mongod.v1)

A simple start/stop struct for *mgo* sessions.

## Install

    go get gopkg.in/go-mong/mongod.v1

## Usage

    m := mongod.New("databaseName")
    db, err := m.Start()
    if err != nil {
      // handle dial error
    }
    defer m.Stop()

    // do mongo stuff

---

On `Stop` callbacks can be performed by passing in a `func(*mgo.Database)`.

    var cleandb = func(t *testing.T) func(*mgo.Database) {
      return func(db *mgo.Database) {
        for _, v := range []string{
          "collection1",
          "collection2",
        } {
          _, err := db.C(v)c.RemoveAll(bson.M{})
          if err != nil {
            t.Fatal(err)
          }
        }
      }
    }

    defer m.Stop(cleandb)

## License

MIT
