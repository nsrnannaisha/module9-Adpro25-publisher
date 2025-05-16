**Nama  : Nisrina Annaisha Sarnadi   
NPM   : 2306275960  
Kelas : B**

Reflection
---

### Tutorial A
1.  **How much data your publisher program will send to the message broker in one run?**  
Dalam satu kali eksekusi, program publisher mengirim lima pesan ke message broker RabbitMQ melalui antrian `user_created`. Setiap pesan berisi data bertipe `UserCreatedEventMessage` yang terdiri dari `user_id` dan `user_name`. Dengan menggunakan Borsh serialization, perkiraan ukuran setiap pesan sekitar 20-30 byte sehingga total data yang dikirim adalah sekitar 100-150 bytes. Jumlah ini belum termasuk overhead dari protokol AMQP, namun cukup mewakili besar data utama yang dikirim.
2. **The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?**  
URL `amqp://guest:guest@localhost:5672` digunakan baik oleh publisher maupun subscriber ke broker RabbitMQ yang sama. Guest pertama adalah username untuk autentikasi, guest kedua adalah password untuk login, localhost menunjukkan bahwa broker berjalan di mesin lokal, dan 5672 adalah port default untuk komunikasi AMQP. Dengan menggunakan URL yang sama, maka publisher mengirim pesan ke broker di localhost dan subscriber menerima pesan dari broker yang sama.  

**Running RabbitMQ**  
![alt text](<image/Running RabbitMQ.png>)  

**Sending and processing event**  
![alt text](image/sending.png)  
Dalam percobaan ini, saya menjalankan dua aplikasi, yaitu subscriber dan publisher. Aplikasi subscriber dijalankan, baru kemudian publisher. Keduanya saling berkomunikasi menggunakan protokol AMQP melalui RabbitMQ sebagai perantara (message broker).  

Saat perintah `cargo run` dijalankan di publisher, program akan mengirimkan sebanyak 5 event bertipe `UserCreatedEventMessage` ke message broker. Setiap event membawa data berupa user ID dan user name. Subscriber yang telah aktif sebelumnya akan langsung menerima dan memproses setiap event yang masuk. Hal ini dapat dilihat pada console subscriber yang mencetak pesan “Message received” untuk tiap event yang diterima.  

Selain itu, tampilan di RabbitMQ melalui browser juga menunjukkan adanya koneksi yang aktif dari subscriber ke broker, yang membuktikan bahwa proses komunikasi antarkomponen berjalan dengan baik.  

**Monitoring chart based on publisher**  
![alt text](image/Monitoring.png)  
Saat `cargo run` di jalankan di publisher, pada bagian Overview RabbitMQ, terdapat grafik Message rates yang menunjukkan adanya lonjakan (spike) aktivitas pengiriman pesan.  

Spike tersebut muncul karena publisher mengirimkan beberapa event (`UserCreatedEventMessage`) ke message broker secara berurutan. RabbitMQ mencatat lonjakan itu sebagai peningkatan jumlah pesan yang dipublish dalam waktu singkat. Hal ini membuktikan bahwa publisher berhasil mengirim pesan dan RabbitMQ aktif memprosesnya.  