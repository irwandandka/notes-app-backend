Deploy Web Services
Setelah selesai membuat RESTful API, tak afdal rasanya bila hanya diakses pada komputer Anda saja. Dapat diakses di mana saja merupakan sifat esensi bagi web server. Untuk melakukan hal itu, kita perlu melakukan proses deployment atau merilis aplikasi ke server yang dapat diakses secara publik.

Pada materi kali ini, kita akan membahas bagaimana cara deployment RESTful API yang sudah kita buat ke Amazon Elastic Compute Cloud atau sering dikenal dengan Amazon EC2. Jadi, di akhir modul ini diharapkan Anda dapat:

Mengetahui Amazon EC2.
Mendaftar Akun AWS.
Membuat dan Menjalankan Amazon EC2 Service.
Mengoperasikan Amazon EC2 melalui SSH.
Menjalankan RESTful API di Amazon EC2.

Sudah siap? Ayo kita mulai!

=======================================================================================================================

Amazon Elastic Compute Cloud
Amazon Elastic Compute Cloud atau Amazon EC2 merupakan salah satu layanan komputasi elastis di cloud yang ditawarkan oleh AWS (Amazon Web Services). Tunggu, apa maksudnya? Komputasi elastis? Cloud? Apa itu? Oke, mari kita mulai secara perlahan.

Sederhananya, EC2 merupakan sebuah komputer server yang dapat Anda miliki namun tidak dapat Anda lihat fisiknya. Walaupun tak tampak secara fisik, komputer ini tetap bisa Anda operasikan di mana saja karena ia disimpan di awan (cloud) yang notabene awan dapat dilihat di mana saja. Namun jangan salah kaprah yah, cloud di sini hanya sebatas istilah. Sebenarnya, EC2 disimpan di Data Center AWS dengan infrastruktur jaringan yang sangat kuat sehingga server dapat diakses secara global dengan sangat cepat.

Selain itu, komputer server ini bersifat elastis karena ia dapat menyesuaikan kapasitasnya berdasarkan permintaan dari client. Semakin banyak permintaan yang datang, semakin besar pula kapasitas server. Dengan begitu, server Anda tidak akan mengalami down jika tiba-tiba bisnis Anda membeludak. Mengapa EC2 bisa seelastis itu? Ini karena EC2 sejatinya adalah virtual komputer atau virtual machine yang dapat diatur spesifikasinya melalui sebuah sistem tanpa harus berupaya dengan perangkat keras. Itulah mengapa ia disebut komputasi elastis.

Banyak sekali keuntungan menggunakan server di cloud dibandingkan dengan server tradisional (on-premise). Salah satu yang paling penting tentu masalah biaya. Ketika menggunakan server tradisional, percayalah, Anda akan mengeluarkan biaya yang besar! Anda perlu membeli server fisik, biaya pengantaran, menyediakan akses internet yang cepat, menyiapkan ruangan yang dingin, biaya listrik, dan biaya operasional lain. Belum lagi bila ada kerusakan perangkat keras, Anda perlu menggantinya dan mengeluarkan biaya lagi.

Beda bila Anda menggunakan komputasi cloud. Anda tidak perlu memikirkan hal-hal yang sudah disebutkan sebelumnya, semua itu menjadi tanggung jawab AWS. Sehingga Anda bisa fokus untuk mengembangkan web server agar pendapatan bisnis bisa lebih berkembang. Biaya untuk menggunakan Amazon EC2 server (instance) bersifat pay-as-you-go, artinya Anda hanya membayar sesuai dengan pemakaian. Jika instance tersebut tidak berjalan, maka tidak dikenakan tagihan.

Kabar gembira untuk Anda yang mau memulai menggunakan Amazon EC2! AWS memberikan Free Tier atau akses gratis selama 12 bulan untuk menggunakan Amazon EC2. Karena itu, ayo kita coba luncurkan Amazon EC2 instance pertama Anda untuk men-deploy web server yang sudah kita buat.

=======================================================================================================================

Sebelum meluncurkan EC2 instance pertama Anda, tentu Anda perlu membuat akun di AWS terlebih dahulu. Di latihan ini, kita akan belajar bagaimana cara membuat akun di AWS. Silakan simak latihannya secara detail yah.

Tahukah Anda? Akun baru akan mendapatkan fasilitas free tier dari AWS. Sehingga kita dapat menggunakan beberapa services di AWS secara gratis selama 12 bulan. Untuk mendaftar di AWS, dibutuhkan kartu kredit/debit yang valid, namun ini hanya digunakan untuk validasi identitas saja. Kartu kredit/debit Anda tidak akan dikenakan tagihan sampai nanti Anda memutuskan untuk meng-upgrade akun menjadi full-subscription.

Penting: Saat mendaftar akun AWS, Anda harus menyediakan saldo 1 dollar di kartu kredit/debit Anda. Biaya 1 dollar ini hanya bersifat sementara dan berfungsi untuk otorisasi kartu saja. Saldo 1 dollar tersebut akan dikembalikan lagi ke akun Anda dalam waktu beberapa hari. Jadi, siapkan kartu debit/kredit di samping Anda, ayo kita mulai buat akun AWS.
Langkah pertama, buka web browser Anda dan kunjungi Homepage AWS.

=======================================================================================================================

Identity and Access Management (IAM)
Kita sudah berhasil membuat akun AWS. Namun, tahukah Anda bahwa akun tersebut merupakan akun yang memiliki kekuatan super atau root user? Yang artinya, akun tersebut dapat mengakses seluruh resource atau service di AWS secara penuh tanpa ada batasan apa pun. 

Root user terbentuk saat pertama kali kita membuat akun AWS. Tentu, kredensial yang digunakan adalah kredensial yang kita masukkan ketika mendaftar akun AWS. Ingatlah! Root user itu memiliki akses yang mutlak dan tidak dapat dibatasi. Oleh karena itu, AWS sangat merekomendasikan kita agar tidak menggunakan root user atau kredensialnya untuk interaksi sehari-hari. Lantas, apa fungsi dari akun yang sudah kita buat sebelumnya?

Praktik terbaiknya, root user hanya digunakan satu kali saja. Gunakan user ini untuk membuat IAM (Identity and Access Management) user atau akun lain yang lebih rendah haknya. Tindakan ini diperlukan guna menghindari Anda atau tim Anda mengakses resource yang tidak atau belum dibutuhkan, bahkan tidak boleh diakses.

AWS Services sangatlah luas, Anda sudah mengenal itu pada kelas Cloud Practitioner Essentials kami bukan? Sebagai developer, mungkin kita hanya perlu berinteraksi dengan beberapa service saja. Misalnya, Amazon EC2, Amazon RDS, atau Amazon ElastiCache.

Bahkan sebagai developer pemula, service yang disebutkan di atas masih terlalu banyak untuk kebutuhan kita sekarang. Ingat! Salah satu prinsip yang ada di AWS adalah “principle of least privilege”. Maksudnya, berikanlah akses sesuai kebutuhan saat itu juga. Kebutuhan apa yang kita perlukan saat ini? Yap! Amazon EC2. Jadi, ayo kita buat IAM users yang memiliki akses Amazon EC2 Instance.

=======================================================================================================================

Membuat IAM Users
Untuk mulai membuat IAM users, silakan kunjungi kembali Homepage AWS. Klik tombol Sign In to the Console. Pilih opsi masuk menggunakan Root user, tuliskan email akun AWS Anda, dan pilih tombol Next. Selamat datang di AWS Management Console! Sebelum membuat IAM Users, ada beberapa hal yang perlu diketahui terlebih dahulu. Silakan lihat di bagian pojok kanan atas, lebih tepatnya informasi di sebelah support. Anda bisa melihat di sana terdapat tulisan Ohio.

Informasi tersebut merupakan Region yang kita gunakan. Region di AWS merupakan letak geografis yang digunakan untuk menyimpan beberapa data center (Availability Zone). Saat ini kita menggunakan Region Ohio dalam menggunakan services AWS. Karena Ohio (Amerika Serikat) letaknya cukup jauh dengan Indonesia, kita akan mengubah Region ke yang lebih dekat agar mendapatkan latensi rendah. Untuk mengubahnya, silakan klik teks “Ohio” dan pilih Regionnya menjadi “Asia Pacific (Singapore) ap-southeast-1”.

Nice! Sekarang kita sudah menggunakan Region paling dekat dengan Indonesia.
Catatan: Anda juga bisa memilih region ap-southeast-3 (Jakarta) yang barus saja hadir. Namun, kami rekomendasikan Anda tetap gunakan region ap-southeast-1 (Singapore) agar selaras dengan modul pembelajaran kelas ini. Yuk, sekarang kita mulai buat IAM Users. Silakan klik All services, kemudian lihat kategori Security, Identity, & Compliance.

Pilih menu IAM. 

Setelah itu, Anda akan diarahkan pada IAM dashboard. Lihat panel samping kiri, pilih menu Users. Kemudian klik tombol Add user. Anda akan diarahkan ke halaman pembuatan user baru.
Silakan berikan nilai:

User name : <isi dengan username yang Anda inginkan>
Access type : AWS Management Console access.
Console password : Custom password. Kemudian isi password Anda.
Require password reset : uncheck. 

Setelah semua terisi, klik Next: Permissions.

Di halaman ini, Anda disuguhkan dengan tiga opsi pemilihan permission. Mari kita jabarkan.

Add user to group : memilih opsi permission dengan cara memasukkan user ke group. Group ini berisi serangkaian permission yang dapat diterapkan ke seluruh user yang berada di dalam grup tersebut. Ini adalah cara terbaik untuk mengelola user permission berdasarkan “job” atau peran dari user tersebut.
Copy permission from existing user : memilih opsi permission dari IAM user lain yang ada. Sayangnya, saat ini kita belum membuat IAM user sama sekali. Jadi opsi ini tidak bisa kita gunakan.
Attach existing policies directly : memilih opsi permission dengan cara langsung menunjuk permission apa saja yang berhak user akses. Opsi ini bisa kita gunakan, namun akan sulit untuk mengelola permission ke depannya.

Berdasarkan pilihan yang ada, kita akan memilih opsi pertama. Namun untuk menggunakannya, kita perlu membuat user group terlebih dahulu. Silakan klik Create group. Isi Group name dengan “Developer”. Kemudian berikan permission untuk AmazonEC2FullAccess. Anda bisa memanfaatkan search box untuk mencari permission-nya. Setelah semuanya terisi dan terpilih dengan benar, klik Create group. Lanjut klik Next: Tags.

Pada halaman ini, kita bisa memberikan tag atau label pada user. Tag ini berisi informasi tambahan yang perlu diberikan pada user yang hendak dibuat. Informasi dapat berupa nama, alamat email, ataupun jabatan. Saat ini, kita tidak perlu memberikan informasi tambahan apa pun. Jadi cukup klik Next: Review. Selanjutnya, Anda akan diarahkan ke halaman review pembuatan user. Di sini Anda bisa meninjau seperti apa nilai-nilai pada user yang akan dibuat. Jika semua sudah sesuai, klik Create User.

Voila! User berhasil dibuat. Silakan unduh .csv untuk mendapatkan kredensial user baru. Gunakan kredensial tersebut untuk masuk ke AWS Management Console. Simpanlah berkas .csv pada tempat yang aman. Untuk melihat kredensialnya, Anda bisa buka berkas .csv yang diunduh dengan menggunakan Notepad ataupun Text Editor lainnya. Seperti yang Anda lihat pada gambar di atas, Anda akan mendapatkan username dan console login link. Console login link bersifat dinamis berdasarkan account id AWS Anda (yang diberikan coretan hitam).

Untuk login menggunakan IAM user, silakan logout dari root user Anda. Kemudian kunjungi alamat login link yang terdapat pada berkas .csv. Isilah username dan password IAM user sesuai yang Anda buat. Kemudian klik Sign in. Well Done! Anda berhasil masuk dengan akun IAM User. Selanjutnya kita bisa mulai membuat dan menjalankan Amazon EC2.

=======================================================================================================================

Membuat dan Menjalankan Amazon EC2 Instance
Di latihan sebelumnya, Anda sudah berhasil membuat Akun IAM yang memiliki full akses untuk services Amazon EC2. Dengan begitu, sekarang Anda layak untuk menggunakan layanan Amazon EC2.

Untuk membuat EC2 Instance, silakan akses menu All Services -> Compute -> EC2. Atau, Anda bisa juga mencarinya menggunakan kolom pencarian yang ada di bar atas. Setelah memilih menu EC2, maka Anda akan diarahkan ke halaman utama service EC2. Gulir jendela browser ke bawah dan Anda akan melihat tombol Launch Instance. Klik tombol tersebut dan pilih Launch instance untuk membuat EC2 instance. Selanjutnya Anda akan diarahkan ke halaman Launch an instance. Silakan isi formulir yang tersedia dengan nilai dan instruksi berikut.

Name and tags: Web Server
Application and OS Images: Ubuntu Server (LTS terbaru) Free tier eligible.
Instance type: t2.micro (Free tier eligible)
Key pair (login):
    Klik Create new key pair.
    Key pair name: notes-api-webserver.
    Klik Create key pair.
    Pastikan berkas notes-api-webserver.pem terunduh.
    Simpan berkas pem yang terunduh pada lokasi yang aman. Karena berkas ini digunakan ketika hendak mengoperasikan instance melalui SSH, jadi jangan sampai hilang yah. Agar mudah, taruh saja berkas tersebut di dalam folder proyek notes-app-back-end.
Network settings:
    Klik Edit.
    Security group name: app-server-sg.
    Description: Allow custom TCP port 5000
    Klik Add security group rule.
    Isi security group baru dengan nilai:
    Type: Custom TCP
    Port range: 5000
    Source type: Anywhere
    Description: Application Port

Klik Launch Instance.
Voila! EC2 instance berhasil dibuat dan berjalan! Anda bisa melihat statusnya dengan mengeklik tombol View Instances.

Penjelasan: Di bagian Network settings kita menentukan cara atau jalur apa saja untuk mengakses EC2 Instance yang dibuat. Secara default, AWS menambahkan SSH pada security group. Jalur SSH ini digunakan nanti oleh kita untuk mengoperasikan EC2 secara remote. Selain SSH, kita juga menambahkan security group lainnya agar web server kita dapat diakses secara publik. Yakni membuka TCP port 5000 (sesuai dengan yang digunakan oleh web server), dan source-nya Anywhere. Anda bisa lihat status dari instance tersebut adalah Running. Selanjutnya kita akan coba mengoperasikan instance melalui SSH.

=======================================================================================================================

Mengoperasikan EC2 Instance Melalui SSH
Setelah EC2 instance terbuat dan berjalan, kini saatnya kita menggunakannya. Untuk mengoperasikan EC2 instance, seperti yang sudah Anda ketahui, kita akan menggunakan protokol SSH. Bagaimana caranya? Yuk disimak.

Kembali ke halaman EC2 instance. Pada halaman daftar instance, pilih instance yang baru saja kita buat. Pilih tombol Connect yang berada di atas, Anda akan diarahkan ke halaman Connect to Instance. Di sini Anda bisa melihat beberapa opsi dalam mengakses EC2 Instance. Untuk mendapatkan informasi bagaimana mengakses melalui SSH, pilih tab SSH client. Silakan salin perintah yang berada di example, lalu paste perintah tersebut pada Terminal/PowerShell/CMD di folder tempat Anda meletakkan berkas pem. Tekan Enter untuk mengeksekusi perintah.

Whops! Ada eror. Dari pesan eror tersebut menjelaskan bahwa berkas notes-api-webserver.pem miliki permission yang terlalu terbuka. Ini tidak aman untuk digunakan sebagai kunci dalam mengakses EC2 instance. Solusinya, kita perlu mengubah permission pada berkas .pem tersebut. Silakan eksekusi perintah berikut (sesuaikan dengan sistem operasi yang Anda gunakan).

$path = ".\notes-api-webserver.pem"
# Reset to remove explicit permissions
icacls.exe $path /reset
# Give current user explicit read-permission
icacls.exe $path /GRANT:R "$($env:USERNAME):(R)"
# Disable inheritance and remove inherited permissions
icacls.exe $path /inheritance:r

Kemudian coba ulangi perintah yang sebelumnya ada pada example. Tada! Kita berhasil terhubung dengan EC2 instance melalui SSH. Anda bisa bebas mengoperasikan instance layaknya komputer Anda sendiri. Namun, komputer ini tak bisa dioperasikan menggunakan mouse karena tidak ada graphical interface-nya. Semua operasi dilakukan menggunakan command line. Jika Anda belum familier dengan perintah-perintah dasar command line yang ada di Linux, sempatkan waktu untuk belajar dasar-dasar command line yang ada yah.

Untuk keluar dari EC2 instance, Anda bisa gunakan perintah exit. Untuk masuk kembali ke EC2 instance, gunakan perintah yang sama seperti sebelumnya.

=======================================================================================================================

Mengunggah Proyek Web Server ke Github
Di latihan sebelumnya Anda sudah berhasil mengoperasikan EC2 instance menggunakan SSH. Namun kali ini kita tinggalkan sejenak Amazon EC2.

Sebelum kita bermain lebih jauh di EC2 instance yang sudah dibuat. Kita perlu mengunggah proyek Web Server ke Internet agar nantinya dapat diunduh dan dijalankan oleh EC2 instance. Dalam proses tersebut, kita akan memanfaatkan teknologi Git dan Github. Banyak developer yang sudah familier dengan dua hal ini. Namun bila Anda baru mendengarnya, jangan khawatir. Kita akan mulai dari awal.

Git merupakan sebuah sistem yang membantu developer dalam melakukan versioning atau source code management terhadap aplikasi yang dikembangkan. Melalui Git, kita dapat menelusuri perubahan kode, siapa yang melakukan perubahannya, hingga mengelola code versioning (branching). Tools ini kerap digunakan sebagai tools berkolaborasi antar developer.

Tapi tunggu, jangan terlalu pusing mendalami tentang git sekarang. Kita tidak akan menggunakan seluruh fungsi yang ada di git. Kita akan fokus untuk mengunggah proyek (dalam git proyek dikenal dengan istilah repository) kita ke internet menggunakan git ini. Jika Anda tertarik mengenal git secara lebih dalam, silakan akses kelas Belajar Dasar Git dengan GitHub yang kami sediakan.

Ketahuilah bahwa sistem git ini hanya berjalan di lokal komputer Anda saja. Agar repository git dapat diakses di mana saja, oleh siapa saja, dan komputer mana saja (termasuk EC2), kita perlu menyimpan repository-nya di internet. Nah, di sinilah peran dari GitHub. Ia merupakan salah satu vendor yang bergerak di bidang source code hosting menggunakan teknologi git. Jadi, ayo kita mulai pasang Git dan inisialisasi repository pada proyek web server kita.

=======================================================================================================================

Memasang Git pada Komputer
Bila komputer Anda sudah terpasang git. Anda bisa melewati proses instalasi dan melanjutkan ke proses inisialisasi repository.

=======================================================================================================================

Menginisialisasi Local Repository pada Proyek Web Server
Setelah memasang git pada komputer, mari kita inisialisasikan local repository pada proyek web server. Silakan buka kembali proyek notes-app-back-end dengan VSCode. Buka Terminal pada proyeknya, lalu tuliskan perintah berikut: Sekarang, setiap perubahan pada berkas yang ada akan dipantau oleh git. Tapi sebenarnya, tidak semua berkas yang ada di dalam proyek harus dipantau oleh git. Contohnya berkas node_modules dan notes-api-webserver.pem.

Berkas yang ada di dalam node_modules tidak perlu dipantau perubahannya karena berkas tersebut memiliki ukuran yang sangat besar. Selain itu berkas node_modules bisa kita peroleh kembali dengan cara menjalankan perintah npm install pada Terminal proyek. Jadi kita tidak membutuhkan node_modules pada repository.

Kemudian untuk notes-api-webserver.pem, berkas ini tentu tidak boleh disimpan di internet nantinya. Ia harus tetap berada di lokal komputer kita karena ini menjadi kunci untuk mengakses EC2 instance melalui SSH. Bila berkas ini terpublikasi, kemungkinan orang di luar sana dapat mengakses EC2 instance secara ilegal. Agar git tidak memproses kedua berkas tersebut, buatlah berkas konfigurasi git dengan nama .gitignore. Di dalamnya tuliskan kode berikut:

node_modules
notes-api-webserver.pem 

Simpan berkas .gitignore dan lihat sekarang berkas node_modules dan notes-api-webserver.pem berubah menjadi abu-abu di VSCode. Ini tandanya, perubahan pada berkas tersebut tidak akan dipantau lagi oleh git. Selanjutnya, tuliskan perintah berikut di Terminal proyek:

git add .
git commit -m "initial commit"

Berikut fungsi dari kedua perintah tersebut:

git add . digunakan untuk memasukan semua berkas ke stash, terkecuali berkas node_modules dan notes-api-webserver.pem.
git commit -m “initial commit” digunakan untuk menyimpan perubahan pada local repository. Setiap perubahan pada sistem git dapat ditelusuri berdasarkan commit history.
Done! Local repository Anda sudah siap. Sekarang kita bisa unggah repository ini ke GitHub.

=======================================================================================================================

Mengunggah Local Repository ke GitHub
Untuk menggunakan GitHub, pastikan Anda sudah memiliki akun. Silakan login dengan akun yang sudah Anda miliki. Namun jika belum, mari kita daftar terlebih dahulu.

Mendaftar Akun Github
Untuk mendaftar akun GitHub, silakan kunjungi halaman utama website GitHub.

=======================================================================================================================



=======================================================================================================================



=======================================================================================================================



=======================================================================================================================



=======================================================================================================================



=======================================================================================================================
