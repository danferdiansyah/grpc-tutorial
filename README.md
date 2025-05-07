<div align="center">
  <h1>Tutorial Module 08</h1>
  <h2>Daniel Ferdiansyah - 2306275052</h2>
</div>

---

**1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?**

Unary RPC adalah jenis RPC paling sederhana, di mana client mengirim satu request dan menerima satu response, seperti method `process_payment` di `PaymentService`.
Lalu, Server streaming merupakan konsep yang memungkinkan server mengirim banyak response untuk satu request client, seperti `get_transaction_history` di `TransactionService`, yang mengirim banyak transaksi ke client.
Sedangkan, Bi-directional streaming merupakan konsep yang memungkinkan client dan server saling bertukar banyak pesan secara asynchronous, seperti method `chat` di `ChatService`. 

**2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?**

* Authentication: Bisa menggunakan mTLS (mutual TLS) untuk verifikasi identitas client dan server.
* Authorization: Bisa menerapkan role-based atau attribute-based access control untuk membatasi akses.
* Data Encryption: Bisa menggunakan TLS sebagai enkripsi komunikasi untuk melindungi data sensitif.
* Input Validation: Sanitasi dan validasi semua input untuk mencegah serangan injection.
* Rate Limiting: Bisa menerapkan rate limiting untuk mencegah penyalahgunaan dan serangan DoS.

**3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?**

* Backpressure Handling: Perlu pengelolaan *chat flow* untuk mencegah overload memory.
* Message Ordering: Pesan bisa datang tidak berurutan jika tidak ditangani dengan baik.
* Error Handling: Diskoneksi atau perilaku client yang unexpected bisa sulit ditangani.
* Concurrency: Mengelola banyak stream secara simultan bisa menambah kompleksitas.

**4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?**

Advantages:

* API sederhana untuk mengkonversi `mpsc::Receiver` menjadi stream.
* Ringan dan efisien karena menggunakan async runtime `tokio`.
* Mudah diintegrasikan ke dalam implementasi gRPC yang ada.

Disadvantages:

* Kapasitas buffer terbatas, bisa menjadi bottleneck pada load tinggi.
* Perlu penanganan yang lebih hati-hati untuk menghindari deadlock atau kehilangan pesan.

**5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?**

* Pisahkan concerns: Bagi services menjadi modul terpisah.
* Gunakan traits dan generics untuk logika yang bisa digunakan ulang.
* Abstraksikan *common pattern* seperti logging dan error handling.
* Modularisasi *business logic* untuk mengurangi coupling.
* Gunakan dependency injection jika diperlukan.

**6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?**

* Validasi request untuk field yang diperlukan dan jumlah pembayaran.
* Integrasi dengan payment gateway untuk transaksi real.
* Implementasi mekanisme rollback untuk kasus kegagalan transaksi.
* Tambahkan logging untuk tracing transaksi.

**7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?**

* Strongly-typed schema melalui Protocol Buffers bisa membantu meningkatkan konsistensi.
* Dukungan HTTP/2 memungkinkan multiplexing dan latency rendah.
* Serialisasi biner yang lebih efisien dibandingkan JSON.
* Dukungan cross-language untuk sistem poliglot.

**8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?**

Advantages:

* Latency lebih rendah melalui multiplexed streams.
* Kompresi header yang lebih efisien (HPACK).
* Kontrol *flow* dan prioritas bawaan.

Disadvantages:

* Implementasi server lebih kompleks.
* Tidak sepopuler HTTP/1.1 atau WebSocket di browser.

**9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?**

REST secara inheren request-response, yang lebih sederhana tapi kurang real-time. gRPC dengan bi-directional streaming memungkinkan komunikasi lebih interaktif dan real-time, cocok untuk chat apps atau live dashboards.

**10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?**

* Strict schemas di gRPC meningkatkan integritas data dan enforcement contract.
* Serialisasi dan deserialisasi lebih cepat.
* Kurang fleksibel dibandingkan JSON, yang lebih mudah beradaptasi dengan perubahan struktur data.
