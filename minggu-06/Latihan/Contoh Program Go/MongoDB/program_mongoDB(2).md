## Program Go MongoDB(Menampilkan data dari database)

### Kode Program

    package main

    import (
	"context"
	"fmt"
	"log"

	"go.mongodb.org/mongo-driver/bson"
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
    func find() {
	db, err := connect()
	if err != nil {
		log.Fatal(err.Error())
	}

	csr, err := db.Collection("mahasiswa").Find(ctx, bson.M{"nim": 175610077})
	if err != nil {
		log.Fatal(err.Error())
	}
	defer csr.Close(ctx)

	result := make([]mahasiswa, 0)
	for csr.Next(ctx) {
		var row mahasiswa
		err := csr.Decode(&row)
		if err != nil {
			log.Fatal(err.Error())
		}

		result = append(result, row)
	}

	if len(result) > 0 {
		fmt.Println("NIM  : ", result[0].NIM)
		fmt.Println("Nama : ", result[0].Nama)
		fmt.Println("Jurusan : ", result[0].Jurusan)
	}
    }

    func main() {
	find()
    }




**Hasil :**

>![Hasil](https://github.com/defri-surya/tekn-cloud-computing/blob/master/Minggu-06/Latihan/Contoh%20Program%20Go/MongoDB/mongodb(2).png)