ESLint
Tools yang kedua adalah ESLint, ia dapat membantu atau membimbing Anda untuk selalu menuliskan kode JavaScript dengan gaya yang konsisten. Seperti yang Anda tahu, JavaScript tidak memiliki aturan yang baku untuk gaya penulisan kode, bahkan penggunaan semicolon. Karena itu, terkadang kita jadi tidak konsisten dalam menuliskannya.

ESLint dapat mengevaluasi kode yang dituliskan berdasarkan aturan yang Anda terapkan. Anda bisa menuliskan aturannya secara mandiri atau menggunakan gaya penulisan yang sudah ada seperti AirBnb JavaScript Code Style, Google JavaScript Code Style, dan StandardJS Code Style. Kami sarankan Anda untuk mengikuti salah satu code style yang ada. Mengapa begitu? Jawabannya karena code style tersebut sudah banyak digunakan oleh JavaScript Developer di luar sana.

Untuk menggunakan ESLint, pasanglah package ESLint pada devDependencies proyek Anda. Caranya, silakan eksekusi perintah berikut di Terminal:

npm install eslint --save-dev

Sebelum digunakan, Anda perlu melakukan konfigurasi terlebih dahulu. Caranya dengan menggunakan perintah berikut di Terminal proyek.

npx eslint --init

Kemudian Anda akan diberikan beberapa pertanyaan, silakan jawab pertanyaan yang ada dengan jawaban berikut:

How would you like to use ESLint? -> To check, find problems, and enforce code style.
What type of modules does your project use? -> CommonJS (require/exports).
Which framework did you use? -> None of these. 
Does your project use TypeScript? -> N.
Where does your code run? -> Node (pilih menggunakan spasi).
How would you like to define a style for your project? -> Use a popular style guide.
Which style guide do you want to follow? -> (Anda bebas memilih, sebagai contoh pilih AirBnB).
What format do you want your config file to be in? -> JSON.
Would you like to …… (seluruh pertanyaan selanjutnya) -> Y.

Setelah menjawab seluruh pertanyaan yang diberikan, maka akan terbentuk berkas konfigurasi eslint dengan nama .eslintrc.json.

Setelah membuat konfigurasi ESLint, selanjutnya kita gunakan ESLint untuk memeriksa kode JavaScript yang ada pada proyek. Namun sebelum itu, kita perlu menambahkan npm runner berikut di dalam berkas package.json:

Jalankan perintah npm run lint pada Terminal proyek, lalu perhatikan hasilnya.

Struktur Proyek
Pada pengembangan web server kali ini, kita tidak ingin semua kode dituliskan dalam satu berkas saja sebab itu akan membuat kode menjadi semrawut, susah dibaca, apalagi dipelihara. Karena Anda sudah belajar teknik modularisasi pada Node.js, tentu tak ada masalah untuk memisahkan kode JavaScript menjadi beberapa berkas.

Kami memegang prinsip single responsibility approach. Artinya, kita gunakan satu berkas JavaScript untuk satu tujuan saja. Nah, di proyek kali ini, kita akan membuat setidaknya empat buah berkas JavaScript. Apa saja berkas dan kode yang dituliskan di dalamnya? Mari kita rincikan.

server.js : Memuat kode untuk membuat, mengonfigurasi, dan menjalankan server HTTP menggunakan Hapi.
routes.js : Memuat kode konfigurasi routing server seperti menentukan path, method, dan handler yang digunakan.
handler.js : Memuat seluruh fungsi-fungsi handler yang digunakan pada berkas routes.
notes.js : Memuat data notes yang disimpan dalam bentuk array objek.

Semua berkas JavaScript yang kita buat akan disimpan di dalam folder src. Hal ini bertujuan agar terpisah dari berkas konfigurasi proyek seperti .eslintrc.json, package.json, package-lock.json, dan node_modules.

Membuat HTTP Server

npm install @hapi/hapi
npm install nanoid

=======================================================================================================================

Same-Origin Policy
Server dapat menampung sebuah website, aplikasi, gambar, video, dan masih banyak lagi. Ketika server menampung website, mungkin beberapa data gambar, video, stylesheet biasanya diambil dari alamat server lain atau origin yang berbeda. Contohnya stylesheet yang diambil dari Bootstrap CDN ataupun gambar yang diperoleh dari server Unsplash. Hal ini wajar dan biasa dilakukan.

Namun apakah Anda tahu bahwa tidak semua data bisa diambil dari origin yang berbeda? Contohnya data JSON yang didapatkan melalui teknik XMLHTTPRequest atau fetch. Jika website meminta sesuatu menggunakan teknik tersebut dari luar origin-nya, maka permintaan tersebut akan ditolak. Itu disebabkan oleh kebijakan same-origin. Kasus ini terjadi pada aplikasi client dan web server yang kita buat.

Origin terdiri dari tiga hal: protokol, host, dan port number. Origin dari aplikasi client kita adalah

http://notesapp-v1.dicodingacademy.com

Di mana protokolnya adalah http://, host-nya adalah notesapp-v1.dicodingacademy.com, dan port-nya adalah :80 (implisit). Selama aplikasi client mengakses data dari origin yang sama, hal itu dapat dilakukan. Namun bila ada salah satu saja yang berbeda contohnya port 8001, maka permintaan itu akan ditolak. Dengan begitu jelas yah, apa penyebab gagalnya aplikasi client ketika melakukan permintaan ke web server yang kita buat. Sudah jelas keduanya memiliki origin yang berbeda. Origin web server kita saat ini adalah http://localhost:5000/

Lalu, apa solusi agar keduanya dapat berinteraksi? Tenang, untungnya ada mekanisme yang dapat membuat mereka saling berinteraksi. Mekanisme tersebut disebut Cross-origin resource sharing (CORS). Pertanyaannya, bagaimana cara menerapkannya? Cukup mudah! Pada web server, kita hanya perlu memberikan nilai header ‘Access-Control-Allow-Origin’ dengan nilai origin luar yang akan mengkonsumsi datanya (aplikasi client).

const response = h.response({ error: false, message: 'Catatan berhasil ditambahkan' });
response.header('Access-Control-Allow-Origin', 'http://notesapp-v1.dicodingacademy.com');
return response;

Atau Anda bisa menggunakan tanda * pada nilai origin untuk memperbolehkan data dikonsumsi oleh seluruh origin.
response.header('Access-Control-Allow-Origin', '*');

Good news! Penerapannya akan jauh lebih mudah bila Anda menggunakan Hapi. Dengan Hapi, CORS dapat ditetapkan pada spesifik route dengan menambahkan properti options.cors di konfigurasi route. Contohnya seperti ini:
{
  method: 'POST',
  path: '/notes',
  handler: addNoteHandler,
  options: {
    cors: {
      origin: ['*'],
    },
  },
},


Bila ingin cakupannya lebih luas alias CORS diaktifkan di seluruh route yang ada di server, Anda bisa tetapkan CORS pada konfigurasi ketika hendak membuat server dengan menambahkan properti routes.cors. Contohnya seperti ini:
const server = Hapi.server({
  port: 5000,
  host: 'localhost',
  routes: {
    cors: {
      origin: ['*'],
    },
  },
});