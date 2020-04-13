## Program Go MySQL(Menampilkan Nama)

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
    func sqlQuery() {
	db, err := connect()
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	defer db.Close()

	var nim = 175610077
	rows, err := db.Query("select nim, nama, jurusan from mahasiswa where nim = ?", nim)
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	defer rows.Close()

	var result []mahasiswa

	for rows.Next() {
		var each = mahasiswa{}
		var err = rows.Scan(&each.nim, &each.nama, &each.jurusan)

		if err != nil {
			fmt.Println(err.Error())
			return
		}

		result = append(result, each)
	}

	if err = rows.Err(); err != nil {
		fmt.Println(err.Error())
		return
	}

	for _, each := range result {
		fmt.Println(each.nama)
	}
    }
    func main() {
	sqlQuery()
    }

**Hasil :**

>![Hasil](https://github.com/defri-surya/tekn-cloud-computing/blob/master/Minggu-06/Latihan/Contoh%20Program%20Go/MySQL/mysql(1).png)