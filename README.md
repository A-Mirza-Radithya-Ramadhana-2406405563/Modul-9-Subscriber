1. What is amqp?  
AMQP adalah singkatan dari **Advanced Message Queuing Protocol**. AMQP digunakan agar aplikasi bisa saling mengirim pesan melalui perantara yang disebut **message broker**, contohnya RabbitMQ. Pada kode tersebut, program bertindak sebagai **subscriber** yang menunggu pesan dengan event atau queue bernama `user_created`.

2. What does it mean? guest:guest@localhost:5672 , what is the first guest, and whatis the second guest, and what is localhost:5672 is for?  
Bagian `amqp://guest:guest@localhost:5672` adalah alamat koneksi ke message broker. Format umumnya adalah `amqp://username:password@host:port`. Jadi, `guest` yang pertama adalah **username**, sedangkan `guest` yang kedua adalah **password**. `localhost` berarti broker berjalan di komputer sendiri, dan `5672` adalah port default yang biasa digunakan AMQP/RabbitMQ.

## Simulation slow subscriber

![alt text](<images/Screenshot 2026-05-12 185921.png>)

Pada chart pertama, yaitu **Queued messages**, terlihat adanya spike pada garis merah. Spike tersebut menunjukkan bahwa message sempat menumpuk di queue karena publisher dijalankan beberapa kali secara cepat.

Pada percobaan saya, jumlah queued message sempat naik sampai **6**. Hal ini terjadi karena publisher dapat mengirim event ke RabbitMQ lebih cepat daripada subscriber memprosesnya.

Setelah subscriber memproses message satu per satu dan mengirim acknowledgement, jumlah queue kembali turun menjadi **0**. Ini menunjukkan bahwa semua message yang sebelumnya berada di queue sudah berhasil dikonsumsi oleh subscriber.