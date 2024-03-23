Advanced Programming A - Ravie Hasan Abud

1. Commit 1 (Handle-connection, check response) Reflection Notes

Pada method main, TcpListener akan keep track connection pada 127.0.0.1:7878. Setelah itu, akan unwrap setiap stream (connection yang diterima oleh TcpListener). Kemudian, stream tersebut akan dijadikan parameter fungsi handle_connection untuk diproses. Pada method handle_connection, instance BufReader digunakan untuk efisiensi dalam membaca setiap line. Hal ini karena BufReader akan melakukan splitting pada stream of data setiap kali ditemukan line baru. Untuk mendapatkan Stringnya, BufReader akan melakukan map dan unwrap untuk setiap Result (return value dari lines()). Selain itu, BufReader juga akan sadar jika ditemukan error pada hasil readnya. Hasil pemrosesan ini akan disimpan pada variabel http_request. Setelah itu, akan dicetak ke terminal.

2. Commit 2 (Returning HTML) Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)

"HTTP/1.1 200 OK" adalah respon sukses standar (menandakan bahwa HTTP request berhasil diproses dengan status 200 OK). Kemudian, file hello.html akan dibaca dengan method read_to_string pada modul fs. Kemudian, akan di unwrap() untuk mengantisipasi apabila terjadi error. Setelah itu,informasi-informasi yang telah diperoleh sebelumnya (status_line, contents, dan length) akan disimpan sebagai String. Setelah itu, method write_all(...) akan mengirim bytes responsed irectly ke connection. Akan digunakan unwrap() juga untuk handle error. Note that pada tahap ini, jika URL request ditambahkan suffix (misal menjadi http://127.0.0.1:7878/suffix) akan tetap memberikan response berupa page yang sama, yakni hello.html. 