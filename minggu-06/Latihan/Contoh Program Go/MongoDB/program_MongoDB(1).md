## Program Go MongoDB(Memasukkan data ke database)

### Kode Program

    package main

    import (
	"context"
	"fmt"
	"log"

	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
    )

    var ctx = context.Background()

    type mahasiswa struct {
	NIM     int    `bson:"nim"`
	Nama    string `bson:"nama"`
	Jurusan string `bson:"jurusan"`
    }

    func connect() (*mongo.Database, error) {
	clientOptions := options.Client()
	clientOptions.ApplyURI("mongodb://localhost:27017")
	client, err := mongo.NewClient(clientOptions)
	if err != nil {
		return nil, err
	}

	err = client.Connect(ctx)
	if err != nil {
		return nil, err
	}

	return client.Database("db_go"), nil
    }
    func insert() {
	db, err := connect()
	if err != nil {
		log.Fatal(err.Error())
	}

	_, err = db.Collection("mahasiswa").InsertOne(ctx, mahasiswa{175610077, "Defri Surya Wirawan", "Sistem Informasi"})
	if err != nil {
		log.Fatal(err.Error())
	}

	fmt.Println("Insert success!")
    }

    func main() {
	insert()
    }



**Hasil :**

>![Hasil](https://github.com/defri-surya/tekn-cloud-computing/blob/master/Minggu-06/Latihan/Contoh%20Program%20Go/MongoDB/mongodb(1).png)