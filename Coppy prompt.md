




# 
```

Tahap berikutnya adalah membangun Provider Manager.

Jangan membuat endpoint AI terlebih dahulu.

Fokus hanya pada sistem manajemen provider.

Buat struktur berikut jika belum ada:

config/
providers/

Tambahkan file konfigurasi yang mendukung banyak provider AI.

Setiap provider harus memiliki konfigurasi seperti:

- id
- name
- enabled
- baseURL
- apiKeys
- supportedModels
- priority
- timeout

Buat ProviderManager yang bertugas:

- memuat konfigurasi provider saat aplikasi dijalankan
- memvalidasi konfigurasi
- mengambil provider berdasarkan model
- hanya mengembalikan provider yang enabled
- mendukung lebih dari satu provider
- mudah ditambah provider baru tanpa mengubah source code

Jangan hardcode provider di dalam kode.

Semua provider harus dibaca dari file konfigurasi.

Tambahkan service untuk:

- listProviders()
- getProviderByModel(model)
- getEnabledProviders()

Tambahkan logging jika konfigurasi provider tidak valid.

Belum perlu membuat request HTTP ke provider.

Belum perlu membuat Chat Completions.

Belum perlu membuat Responses API.

Belum perlu API Key Rotation.

Belum perlu Streaming.

Setelah selesai tampilkan struktur folder terbaru dan jelaskan desain arsitektur Provider Manager.




```

# 
```

Audit seluruh repository dan perbaiki struktur proyek tanpa mengubah fungsi aplikasi.

Terjadi kesalahan saat pembuatan file. Beberapa teks dari README atau contoh output terminal salah dibuat menjadi file di root project.

Lakukan langkah berikut:

1. Scan seluruh root project.
2. Identifikasi file yang bukan bagian dari proyek, misalnya file yang namanya berupa kalimat, output terminal, atau contoh dokumentasi.
3. Hapus file-file tersebut dengan aman.
4. Pindahkan informasi yang masih berguna ke README.md atau .env.example sesuai fungsinya.
5. Pastikan root project hanya berisi file dan folder yang memang diperlukan.

Root project setelah dirapikan minimal berisi:

.git
src/
config/
logs/
package.json
package-lock.json (jika ada)
README.md
.gitignore
.env.example

Pastikan folder src tetap menggunakan clean architecture.

Jangan menghapus source code yang valid.

Jangan mengubah endpoint, logika aplikasi, atau struktur source code yang sudah benar.

Setelah selesai:

- tampilkan daftar file yang dihapus,
- jelaskan alasannya,
- tampilkan struktur proyek terbaru menggunakan `tree -L 3`,
- pastikan proyek masih dapat dijalankan tanpa error.

Jangan membuat fitur baru. Fokus hanya membersihkan dan merapikan struktur repository.




```
