## Program Go MySQL(Menampilkan Semua Field)

### Kode Program

    package main

    import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"
    )

    type mahasiswa struct {
	nim     string
	nama    string
	jurusan string
    }

    func connect() (*sql.DB, error) {
	db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/db_go")
	if err != nil {
		return nil, err
	}

	return db, nil
    }

    func sqlQueryRow() {
	var db, err = connect()
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	defer db.Close()

	var result = mahasiswa{}
	var id = "1"
	err = db.
		QueryRow("select nim, nama, jurusan from mahasiswa where id = ?", id).
		Scan(&result.nim, &result.nama, &result.jurusan)
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Printf("nim: %s\nnama: %s\njurusan: %s\n", result.nim, result.nama, result.jurusan)
    }

    func main() {
	sqlQueryRow()
    }


**Hasil :**

>![Hasil](https://github.com/defri-surya/tekn-cloud-computing/blob/master/Minggu-06/Latihan/Contoh%20Program%20Go/MySQL/mysql(2).png)