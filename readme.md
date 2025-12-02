# TUGAS 1 

| No | Parameter / Aspek | Field Wireshark | tes1000.bin | tes10000.bin | tes5M.bin | Analisis / Catatan |
|---:|------------------|----------------|------------:|-------------:|----------:|------------------|
| 1 | Ukuran File | — | 1000 B | 10000 B | 5000000 B | Dari hasil curl |
| 2 | Total Segmen TCP | Conversations → TCP | 17 | 26 | 4361 | File lebih besar → segmen lebih banyak |
| 3 | RTT Rata-rata | tcp.analysis.ack_rtt | 0.00118 s | 0.00097 s | 0.00092 s | RTT stabil → jaringan baik |
| 4 | Throughput | curl: speed_download | 2,675,222 B/s | 2,232,641 B/s | 46,932,492 B/s | Throughput optimal di ukuran besar |
| 5 | Waktu Transfer | curl: time_total | 0.003738 s | 0.004479 s | 0.106536 s | Linear terhadap ukuran file |
| 6 | Seq Number Awal Klien | tcp.seq (SYN) | 255683846 | 402021460 | 337448885 | Nilai acak tiap koneksi |
| 7 | Seq Number Awal Server | tcp.seq (SYN-ACK) | 1 | 0 | 0 | Server badan ACK awal |
| 8 | Jumlah ACK Klien | Endpoints → TCP | ±9 | ±13 | ±4294 | Banyak data → banyak ACK |
| 9 | Pola Pertumbuhan SEQ–ACK | tcp.seq / tcp.ack | kecil | sedang | besar | Window membesar (MSS ~1514 B) |
|10 | Retransmission | tcp.analysis.retransmission | Tidak ada | Tidak ada | Ada (1x) | Reliable, minimal loss |


# TUGAS 2 
| File         | Algoritma | RTT Rata-rata (ms) | Retransmission | Throughput (B/s) | Waktu Transfer (s) | cwnd (bytes)       | Pola cwnd                    | Catatan |
|-------------|-----------|------------------|----------------|-----------------|-------------------|-------------------|-----------------------------|--------|
| tes1000.bin | Reno      | 200–400          | 0              | 2487            | 0.402060          | Kecil stabil       | Linear (AIMD)               | Tidak ada loss, transfer sangat cepat |
| tes1000.bin | Cubic     | 200–400          | 0              | 2454            | 0.407437          | Kecil stabil       | Kurva Cubic (agresif)       | Mirip Reno pada ukuran kecil |
| tes10000.bin| Reno      | 200–420          | 0              | 23168           | 0.431614          | Bertahap naik      | Linear (AIMD)               | Stabil tanpa retransmission |
| tes10000.bin| Cubic     | 200–420          | 0              | 23210           | 0.430831          | Lebih agresif naik | Kurva Cubic                 | Throughput sedikit lebih tinggi dari Reno |
| tes5M.bin   | Reno      | 200–440          | Banyak (Fast Retr) | 105842      | 47.240132         | Turun saat loss    | AIMD – sering recovery     | TCP congestion terlihat, performa turun |
| tes5M.bin   | Cubic     | 200–420          | Banyak (Fast Retr) | 98927       | 50.541921         | Tetap agresif      | Pemulihan lebih cepat       | Transfer lebih lambat dari Reno pada kondisi loss |
