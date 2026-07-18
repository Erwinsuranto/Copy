









# Prompt: Images API
```
Implement full OpenAI-compatible Images API.

Requirements:

1. Add endpoints:

POST /v1/images/generations
POST /v1/images/edits
POST /v1/images/variations

Maintain full OpenAI API compatibility.

2. Support request fields:

- model
- prompt
- image
- mask
- n
- size
- quality
- style
- response_format
- user

Validate all requests.

3. Integrate into existing architecture only.

Reuse:

- ProviderManager
- ModelRouter
- RequestExecutor
- ProviderAdapter
- HttpClient
- Retry
- Fallback
- Logging
- ApiKeyManager

Do not duplicate logic.

4. Provider capability

Add supportsImages to ProviderAdapter.

Automatically filter unsupported providers.

Return OpenAI-compatible error if unsupported.

5. Provider support

OpenAI-compatible:

- OpenAI
- OpenRouter
- TokenFaucet (if available)
- Gemini/OpenAI-compatible
- NVIDIA
- Databricks

Providers without image capability must be skipped automatically.

6. Response

Support both:

- url
- b64_json

Normalize all provider responses into OpenAI format.

7. Streaming

Images never use streaming.

Reject stream=true.

8. Retry/Fallback

Reuse existing retry/fallback.

9. Tests

Add integration tests for:

- image generation
- edits
- variations
- unsupported provider
- fallback
- validation
- response normalization
- OpenAI compatibility

10. Documentation

Update README with:

- supported endpoints
- examples
- provider support
- limitations

Keep clean architecture.

No duplicated code.

No provider-specific logic outside ProviderAdapter.

Finish only when all tests pass.
```



# 
```
Implement full OpenAI-compatible Embeddings API.

Requirements:

1. Add endpoint:

POST /v1/embeddings

Compatible with OpenAI request/response format.

2. Request

Support:

- input (string)
- input (array)
- model
- encoding_format
- dimensions (if provider supports)

Validate request.

3. Provider Integration

Use existing architecture:

ProviderManager
ModelRouter
RequestExecutor
ProviderAdapter

Do not duplicate provider logic.

ProviderAdapter must expose embeddings capability.

If provider does not support embeddings, return a proper OpenAI-compatible error.

4. Provider Mapping

OpenAI-compatible providers:

- OpenAI
- OpenRouter
- TokenFaucet
- DeepSeek
- NVIDIA
- Gemini (OpenAI compatible)
- Databricks

Anthropic should return "Embeddings not supported".

5. Response

Return OpenAI-compatible JSON:

{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "embedding": [...],
      "index": 0
    }
  ],
  "model": "...",
  "usage": {
    "prompt_tokens": ...,
    "total_tokens": ...
  }
}

6. Streaming

Embeddings must never use streaming.

7. Retry

Reuse existing retry/fallback system.

8. Logging

Reuse existing logging.

9. Tests

Add integration tests for:

- successful embedding
- multiple inputs
- unsupported provider
- validation errors
- provider fallback
- OpenAI compatibility

10. Documentation

Update README with:

- endpoint
- request examples
- response examples
- provider support table

Do not modify existing architecture.

Keep clean architecture.

Reuse existing abstractions.

Do not duplicate code.

Finish only after all tests pass.
```


# Tahap berikutnya: Integrasi Provider Nyata
```


Tahap berikutnya adalah implementasi Provider Adapter.

Core gateway sudah selesai.

Sekarang fokus membuat adapter provider nyata.

Buat folder:

src/providers/

Setiap provider berada pada file terpisah.

Minimal support:

- OpenAI
- OpenRouter
- TokenFaucet
- Anthropic
- Google Gemini (OpenAI compatible jika tersedia)
- DeepSeek
- Databricks
- NVIDIA

Buat interface provider yang sama untuk seluruh adapter.

Setiap provider bertugas:

- membangun endpoint
- mapping payload jika diperlukan
- mapping response jika diperlukan
- mendeteksi capability provider
- tidak mengandung retry
- tidak mengandung fallback
- tidak mengandung API key rotation

Retry, fallback, logging dan HttpClient tetap berasal dari core gateway.

Tambahkan capability seperti:

supportsChat

supportsResponses

supportsStreaming

supportsEmbeddings

supportsImages

supportsAudio

supportsTools

supportsReasoning

ProviderManager harus otomatis memilih adapter berdasarkan konfigurasi.

Jika provider OpenAI compatible, gunakan GenericOpenAIAdapter.

Provider yang membutuhkan mapping khusus menggunakan adapter sendiri.

Tambahkan integration test menggunakan mock provider.

Tambahkan contoh konfigurasi minimal untuk setiap provider.

Pastikan tidak ada duplikasi kode antar adapter.

Pertahankan clean architecture.



```
# Streaming (Server-Sent Events / SSE)
```

Tahap berikutnya adalah implementasi Streaming (Server-Sent Events / SSE).

Tujuan:

Menambahkan dukungan streaming OpenAI Compatible untuk:

- POST /v1/chat/completions
- POST /v1/responses

Jika request memiliki:

"stream": true

Gateway harus mengembalikan response streaming menggunakan format OpenAI Compatible SSE.

Persyaratan:

- Jangan membuat endpoint baru.
- Reuse seluruh service yang sudah ada.
- Gunakan RequestExecutor yang sama.
- Gunakan HttpClient yang sama.
- Gunakan Retry dan Fallback yang sudah ada.

Flow:

1. Validasi request.
2. Cari provider.
3. Ambil API key.
4. Jika stream=false gunakan flow lama.
5. Jika stream=true gunakan HttpClient stream mode.
6. Forward seluruh SSE event ke client.
7. Jika provider mengirim [DONE], teruskan ke client lalu tutup koneksi.

Tambahkan:

- StreamingResponseAdapter
- StreamParser
- SSEWriter

Logging:

- request id
- provider
- model
- stream started
- stream ended
- latency
- bytes sent

Error:

Jika error terjadi sebelum stream dimulai:
kembalikan JSON OpenAI error.

Jika error terjadi saat stream berlangsung:
kirim event error sesuai format OpenAI lalu tutup stream.

Belum membuat:

- Dashboard
- Database
- Metrics
- Authentication
- Embeddings
- Images
- Audio

Tambahkan integration test menggunakan mock SSE provider.

Tambahkan contoh curl:

curl ... -d '{"stream":true}'

Pastikan implementasi tetap mengikuti clean architecture dan tidak menduplikasi kode non-streaming.




```

# 
```


Tahap berikutnya adalah implementasi OpenAI Responses API.

Tujuan:

Menambahkan endpoint:

POST /v1/responses

dengan tetap menggunakan arsitektur yang sudah ada.

Persyaratan:

- Jangan menduplikasi kode Chat Completions.
- Reuse seluruh komponen yang sudah ada:
  - ProviderManager
  - ModelRouter
  - HttpClient
  - ApiKeyManager
  - Retry
  - Fallback
  - Logger

Responses API harus menjadi adapter di atas service yang sudah ada.

Validasi request:

- model wajib ada
- input wajib ada
- support string maupun array input
- metadata optional
- instructions optional
- temperature optional
- max_output_tokens optional

Flow:

1. Validasi request.
2. Cari provider menggunakan ModelRouter.
3. Ambil API Key dari ApiKeyManager.
4. Kirim request menggunakan HttpClient.
5. Gunakan Retry jika retryable.
6. Gunakan Provider Fallback jika provider gagal.
7. Normalisasi response menjadi format OpenAI Responses API.

Error harus konsisten dengan Chat Completions.

Tambahkan logging:

- request id
- provider
- model
- latency
- retry count
- fallback count

Tambahkan integration test.

Tambahkan contoh curl.

Pastikan seluruh endpoint berikut berbagi service yang sama:

GET /v1/models

POST /v1/chat/completions

POST /v1/responses

Jangan membuat endpoint lain.

Jangan membuat Dashboard.

Jangan membuat Database.

Jangan membuat Authentication.

Jangan membuat Embeddings.

Jangan membuat Images.

Jangan membuat Audio.

Pastikan clean architecture tetap dipertahankan dan tidak ada duplikasi kode.



```

# 
```


Tahap berikutnya adalah membangun API Key Manager.

Jangan membuat Dashboard.

Jangan membuat Responses API.

Jangan membuat Streaming.

Fokus hanya pada sistem manajemen API Key.

Tujuan:

Semua provider dapat memiliki banyak API key.

Contoh:

Provider OpenAI
- key1
- key2
- key3

Provider TokenFaucet
- key1
- key2

Provider OpenRouter
- key1
- key2
- key3
- key4

Buat ApiKeyManager dengan fitur:

- memuat seluruh API key dari konfigurasi
- mendukung banyak API key per provider
- round robin
- next available key
- skip disabled key
- temporary disable key jika gagal
- cooldown key
- re-enable otomatis setelah cooldown selesai

Status API key:

ACTIVE

RATE_LIMITED

UNAUTHORIZED

QUOTA_EXCEEDED

DISABLED

COOLDOWN

Tambahkan service:

getNextKey(providerId)

reportSuccess(providerId,key)

reportFailure(providerId,key,error)

disableKey(providerId,key)

enableKey(providerId,key)

getKeyStatus(providerId)

Tambahkan statistik:

jumlah request

jumlah sukses

jumlah gagal

last used

last error

cooldown until

Belum implementasi retry.

Belum implementasi fallback provider.

Belum implementasi dashboard.

Belum implementasi database.

Semua data masih boleh berada di memory.

Pastikan HttpClient menggunakan ApiKeyManager saat mengambil Authorization Bearer.

Tambahkan unit test.

Tambahkan integration test.

Pastikan clean architecture tetap terjaga.

Setelah selesai jelaskan alur kerja ApiKeyManager dan bagaimana nanti akan digunakan oleh Retry dan Fallback.



```


# Prompt: Build Complete Folder Management for Telegram Drive

Tujuan:
Bangun sistem Folder Management yang modern seperti Google Drive, MEGA, dan Dropbox. Semua file harus dapat dikelompokkan ke dalam folder. Sistem harus siap digunakan oleh file yang berasal dari Website maupun Telegram Downloader Bot.

## Folder List

Buat halaman Folder yang menampilkan:

- Folder Card/Grid
- Folder List View
- Nama Folder
- Jumlah File
- Total Ukuran
- Tanggal Dibuat
- Tanggal Update Terakhir
- Icon Folder

## Folder Actions

Saat menekan tombol (⋮) tampilkan:

- Open
- Rename
- Move
- Share
- Favorite
- Delete

## Create Folder

Tambahkan tombol:

+ New Folder

Saat ditekan tampilkan dialog:

- Nama Folder
- Tombol Cancel
- Tombol Create

Validasi:

- Nama tidak boleh kosong.
- Tidak boleh ada folder dengan nama yang sama pada lokasi yang sama.

## Rename Folder

Dialog Rename Folder.

## Delete Folder

Jika folder kosong:
- Hapus langsung setelah konfirmasi.

Jika folder berisi file:
Tampilkan pilihan:

- Pindahkan file ke folder lain
- Hapus seluruh isi folder
- Batal

## Move File

Saat memilih Move pada file:

Tampilkan Folder Picker.

User dapat memilih folder tujuan.

## Breadcrumb

Contoh:

Home
>
My Files
>
Photos
>
Vacation

Breadcrumb harus dapat ditekan.

## Search Folder

Search Folder secara realtime.

## Sort Folder

- Nama A-Z
- Nama Z-A
- Terbaru
- Terlama

## Favorite Folder

Folder dapat ditandai Favorite.

## Empty State

Jika belum ada folder:

Tampilkan ilustrasi.

Pesan:

"Belum ada folder."

"Tekan New Folder untuk membuat folder pertama."

## Loading

Gunakan Skeleton Loading.

## Error

Jika folder tidak ditemukan tampilkan Folder Not Found.

## Mobile

Gunakan Bottom Sheet.

## Desktop

Gunakan Context Menu.

## API

Semua folder menggunakan API.

Jangan menggunakan data dummy.

Gunakan service/repository yang sudah ada.

## Persiapan Telegram Downloader Bot

Saat integrasi selesai:

- File hasil Telegram Downloader Bot dapat langsung disimpan ke folder pilihan user.
- Upload Website juga dapat memilih folder.
- Upload Telegram Bot juga menggunakan struktur folder yang sama.

Folder menjadi struktur utama penyimpanan Telegram Drive.

## Target

Folder Management harus siap dipakai sebagai fondasi Telegram Drive sehingga seluruh file dari Website maupun Telegram Downloader Bot memiliki struktur penyimpanan yang rapi, modern, responsif, dan mudah dikelola.
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
