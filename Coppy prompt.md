







# 
```


Tahap berikutnya adalah implementasi endpoint OpenAI Compatible Chat Completions.

Fokus hanya pada endpoint:

POST /v1/chat/completions

Tujuan:

Menerima request OpenAI Compatible, memilih provider menggunakan Model Router, lalu mengirim request menggunakan HttpClient.

Persyaratan:

- Gunakan ProviderManager.
- Gunakan ModelRouter.
- Gunakan HttpClient.
- Jangan hardcode provider.
- Jangan hardcode model.
- Semua provider berasal dari konfigurasi.

Validasi request:

- model wajib ada
- messages wajib ada
- messages harus berupa array
- minimal satu message

Flow:

1. Validasi request.
2. Cari provider berdasarkan model.
3. Bangun payload OpenAI Compatible.
4. Kirim request menggunakan HttpClient.
5. Kembalikan response provider dalam format OpenAI Compatible.

Belum implementasi:

- Streaming
- Retry
- API Key Rotation
- Fallback Provider
- Responses API
- Embeddings
- Images
- Audio

Tambahkan logging:

- model
- provider
- durasi request
- status code
- request id

Normalisasi seluruh error menjadi format OpenAI Compatible.

Tambahkan integration test menggunakan mock provider.

Tambahkan contoh request curl.

Pastikan endpoint dapat bekerja tanpa mengubah arsitektur yang sudah ada.



```

# 
```


Tahap berikutnya adalah membangun HTTP Client.

Jangan membuat endpoint Chat Completions atau Responses API terlebih dahulu.

Fokus hanya membuat lapisan komunikasi ke provider AI.

Buat HttpClient yang reusable untuk semua provider.

Persyaratan:

- Gunakan axios.
- Semua request menggunakan timeout dari konfigurasi provider.
- Semua header dibangun secara otomatis.
- Mendukung Authorization Bearer.
- Mendukung custom header provider.
- Mendukung POST dan GET.
- Mendukung query parameter.
- Mendukung request body JSON.
- Mendukung streaming di tahap berikutnya (siapkan struktur, tetapi belum implementasi).

Tambahkan:

- Request Logger
- Response Logger
- Error Logger

Normalisasi seluruh error menjadi format internal gateway.

Misalnya:

- Timeout
- 401
- 403
- 404
- 429
- 500
- 502
- 503
- Connection refused
- DNS error
- Invalid JSON

Semua error harus menghasilkan objek error internal yang konsisten.

HttpClient harus dapat dipanggil seperti:

sendRequest(provider, endpoint, payload)

Belum perlu melakukan retry.

Belum perlu API Key Rotation.

Belum perlu Streaming.

Belum perlu Chat Completions.

Belum perlu Responses API.

Tambahkan unit test sederhana atau contoh penggunaan HttpClient.

Pastikan desain mengikuti clean architecture sehingga seluruh provider nantinya menggunakan HttpClient yang sama.



```


# 
```
Tahap berikutnya adalah membangun Model Router.

Jangan membuat request HTTP ke provider terlebih dahulu.

Jangan membuat Chat Completions atau Responses API.

Fokus hanya pada sistem routing model.

Buat komponen Model Router yang bertugas memilih provider berdasarkan model yang diminta.

Persyaratan:

- Model Router menggunakan ProviderManager.
- Tidak boleh ada hardcode provider di dalam router.
- Semua konfigurasi model berasal dari file konfigurasi.
- Mendukung banyak provider.
- Mendukung satu model tersedia di lebih dari satu provider.
- Menggunakan priority provider sebagai urutan pemilihan.
- Jika provider disabled, jangan digunakan.
- Jika model tidak ditemukan, kembalikan error yang jelas.

Buat service berikut:

- getProviderForModel(model)
- hasModel(model)
- listModels()
- listProvidersForModel(model)

Tambahkan validasi:

- model kosong
- model tidak dikenal
- provider tidak aktif
- konfigurasi model rusak

Tambahkan unit helper agar mudah dipakai endpoint nanti.

Belum perlu melakukan HTTP request.

Belum perlu API Key Rotation.

Belum perlu Retry.

Belum perlu Streaming.

Belum perlu Responses API.

Belum perlu Chat Completions.

Tambahkan logging sehingga saat aplikasi dijalankan akan tercetak:

- jumlah provider
- jumlah model
- model yang dimiliki masing-masing provider

Pastikan desain mengikuti clean architecture.

Setelah selesai tampilkan struktur folder terbaru dan jelaskan alur kerja Model Router.





```

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
