Advanced Programming A - Ravie Hasan Abud

1. Commit 1 (Handle-connection, check response) Reflection Notes

Pada method main, TcpListener akan keep track connection pada 127.0.0.1:7878. Setelah itu, akan unwrap setiap stream (connection yang diterima oleh TcpListener). Kemudian, stream tersebut akan dijadikan parameter fungsi handle_connection untuk diproses. Pada method handle_connection, instance BufReader digunakan untuk efisiensi dalam membaca setiap line. Hal ini karena BufReader akan melakukan splitting pada stream of data setiap kali ditemukan line baru. Untuk mendapatkan Stringnya, BufReader akan melakukan map dan unwrap untuk setiap Result. Result yang dimaksud pada adalah return value dari lines(), yaitu berupa Result<String, std::io::Error>. Selain itu, BufReader juga akan sadar jika ditemukan error pada hasil readnya. Hasil pemrosesan ini akan disimpan pada variabel http_request. Setelah itu, akan dicetak ke terminal.

2. Commit 2 (Returning HTML) Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)

"HTTP/1.1 200 OK" adalah respon sukses standar (menandakan bahwa HTTP request berhasil diproses dengan status 200 OK). Kemudian, file hello.html akan dibaca dengan method read_to_string pada modul fs. Kemudian, akan di unwrap() untuk mengantisipasi apabila terjadi error. Setelah itu,informasi-informasi yang telah diperoleh sebelumnya (status_line, contents, dan length) akan disimpan sebagai String. Setelah itu, method write_all(...) akan mengirim bytes responsed irectly ke connection. Akan digunakan unwrap() juga untuk handle error. Note that pada tahap ini, jika URL request ditambahkan suffix (misal menjadi http://127.0.0.1:7878/suffix) akan tetap memberikan response berupa page yang sama, yakni hello.html.

3. Commit 3 (Validating request and selectively responding) Reflection Notes

![Commit 3 screen capture](/assets/images/commit3.png)

Dengan statement if request_line == "GET / HTTP/1.1", block if tersebut hanya akan dieksekusi jika HTTP request yang diterima menggunakan method GET dan mengarah ke URL main ("/"). Otherwise,block else yang akan dieksekusi (HTTP request mengarah ke URL lain atau method yang diterima bukan GET). Kode yang dijalankan pada block if dan else cukup mirip, perbedaannya adalah pada status_line (HTTP requestnya) dan contents (template HTML yang akan digunakan). Jika mengarah ke main page, akan menampilkan hello.html, otherwise akan menampilkan 404.html. Dengan demikian,
jika HTTP request yang diterima ditambahkan suffix (misal menjadi http://127.0.0.1:7878/suffix), akan memberikan response berupa page 404.html. Kemudian, terdepat duplicate code pada block if else karena perbedaannya hanya 2 baris seperti yang telah saya sampaikan sebelumnya, sehingga commit setelah ini akan berisikan refactor untuk hal tersebut.