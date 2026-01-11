# SOC-Workbooks-and-Lookups / Buku-Kerja-dan-Pencarian-SOC
Hallo Semua
Triage peringatan adalah proses kompleks yang seringkali mengharuskan analis untuk mengumpulkan informasi tambahan tentang karyawan atau server yang terpengaruh. Ruangan ini membahas buku kerja SOC yang dirancang untuk menyederhanakan triage peringatan dan menjelaskan berbagai metode pencarian untuk dengan cepat mendapatkan konteks pengguna dan sistem.

Tujuan pembelajaran
Pelajari dan pahami buku kerja investigasi SOC.
Pelajari di mana menemukan dan bagaimana menggunakan inventaris aset di SOC.
Memahami pentingnya diagram jaringan perusahaan
Membangun alur kerja praktik di dalam antarmuka interaktif.
Aset & Identitas
Bayangkan Anda sedang bertugas malam dan memeriksa peringatan yang menyatakan bahwa G.Baker masuk ke server HQ-FINFS-02. Kemudian, pengguna tersebut mengunduh file "Financial Report US 2024.xlsx" dari sana dan membagikannya kepada R.Lund. Untuk mengklasifikasikan peringatan tersebut dengan benar dan memahami apakah aktivitas tersebut diharapkan, Anda harus menemukan jawaban atas banyak pertanyaan:

Siapakah G.Baker? Berapa jam kerja dan peran mereka di perusahaan?
Apa tujuan dan lokasi dari HQ-FINFS-02? Siapa yang dapat mengaksesnya?
Mengapa R.Lund membutuhkan akses ke catatan keuangan perusahaan?
Inventaris Identitas <img width="4476" height="972" alt="image" src="https://github.com/user-attachments/assets/8d6bd3c7-c5ed-4d75-89a6-0e221908d600" />
Inventaris identitas adalah katalog karyawan perusahaan (akun pengguna), layanan (akun mesin), dan detail mereka seperti hak akses, kontak, dan peran dalam perusahaan. Untuk skenario di atas, inventaris identitas akan membantu Anda mendapatkan konteks tentang G.Baker dan R.Lund, dan mempermudah untuk memutuskan apakah aktivitas tersebut diharapkan atau tidak.

Contoh Identitas
Sumber Identitas
Solution	Examples	Description
Direktori Aktif	Active Directory (AD) lokal , Entra ID	Active Directory (AD) sendiri merupakan basis data identitas, dan umumnya digunakan oleh Security Operations Center (SOC).
Penyedia SSO	Okta, Google Workspace	Alternatif cloud untuk Active Directory (AD) , cara mudah untuk mengelola dan mencari pengguna.
Sistem SDM	BambooHR, SAP, HiBob	Hanya terbatas untuk karyawan, tetapi biasanya memberikan data karyawan lengkap.
Solusi Khusus	Lembar CSV atau Excel	Merupakan hal yang umum bagi tim TI atau keamanan untuk memelihara solusi mereka sendiri.
Inventaris Aset<img width="1132" height="401" alt="image" src="https://github.com/user-attachments/assets/ec86155c-e34e-4538-9782-0ed46820cac9" />
Inventaris aset, juga disebut pencarian aset, adalah daftar semua sumber daya komputasi dalam lingkungan TI suatu organisasi. Perlu dicatat bahwa meskipun "aset" adalah istilah yang ambigu dan juga dapat merujuk pada perangkat lunak, perangkat keras, atau karyawan, ruangan ini hanya menekankan server dan workstation. Untuk skenario di atas, inventaris aset akan membantu Anda mendapatkan konteks tentang server HQ-FINFS-02.

Contoh Aset
Sources of Assets

Solution	Examples	Description
Direktori Aktif	Active Directory (AD) lokal , Entra ID	AD bukan hanya sebuah identitas tetapi juga basis data inventaris aset yang solid.
SIEM atau EDR	Elastis, CrowdStrike	Beberapa agen SIEM atau EDR mengumpulkan informasi tentang host yang dipantau.
Solusi MDM	MS Intune, Jamf MDM	Seperangkat solusi khusus yang dibuat untuk mendaftarkan dan mengelola aset.
Solusi Khusus	Lembar CSV atau Excel	Sama seperti inventaris identitas, solusi khusus juga umum digunakan.
Jawablah pertanyaan-pertanyaan di bawah ini.
Berdasarkan inventaris identitas, apa peran R.Lund di perusahaan tersebut?

US Financial Adviser
Dengan memeriksa inventaris aset, data apa yang disimpan oleh server HQ-FINFS-02?
Financial records
Terakhir, apakah berbagi file dari skenario tersebut terlihat sah dan sesuai harapan? (Ya/Tidak)
Yea
Diagram Jaringan
Melanjutkan topik identitas dan inventaris aset, Anda mungkin juga perlu melihat peringatan dari sudut pandang jaringan, terutama di perusahaan yang lebih besar. Pertimbangkan skenario di mana Anda sedang menyelidiki serangkaian peringatan terkait berdasarkan log firewall dan ingin memberikan makna pada IP yang Anda lihat:

08:00 : Sebuah IP 103.61.240.174 berulang kali terhubung ke firewall perusahaan melalui port TCP /10443
08:23 : Log firewall menunjukkan bahwa IP 103.61.240.174 diterjemahkan ke IP internal 10.10.0.53
08:25 : IP 10.10.0.53 sedang memindai rentang jaringan 172.16.15.0/24 tetapi tidak menemukan port yang terbuka.
08:32 : IP yang sama sekarang sedang memindai rentang jaringan 172.16.23.0/24 , dan serangan tampaknya masih berlangsung.
Diagram Jaringan
Untuk menyelidiki kasus di atas, Anda harus mencari tahu layanan apa yang berjalan di port 10443 dan mengapa ada orang yang terhubung ke sana. Kemudian, identifikasi subnet tempat IP 10.10.0.53 berada dan mengapa IP tersebut mencoba terhubung ke subnet lain. Diagram jaringan , skema visual yang menyajikan lokasi, subnet, dan koneksi yang ada, adalah jawaban atas pertanyaan Anda:

<img width="3560" height="1480" alt="image" src="https://github.com/user-attachments/assets/aeaca591-1437-40c3-937a-41372815cd2a" />
Tergantung pada ukuran dan struktur perusahaan, Anda mungkin melihat diagram yang lebih kompleks, tetapi kegunaannya bagi analis SOC tetap sama - untuk membantu memahami aktivitas jaringan yang mencurigakan. Dalam skenario kita, Anda dapat merujuk pada diagram jaringan dan merekonstruksi jalur serangan sebagai berikut:

Pelaku ancaman di balik IP 103.61.240.174 melakukan serangan brute force pada VPN , menargetkan vpn .tryhatme. thm
Setelah serangan brute force dan login VPN yang berhasil , pelaku ancaman diberi alamat IP dari subnet VPN.
Kemudian, pihak lawan mencoba memindai Subnet Basis Data , tetapi kemungkinan diblokir oleh aturan firewall.
Karena tidak menemukan keberhasilan, pelaku ancaman beralih ke Subnet Kantor , mencari target berikutnya.

Jawablah pertanyaan-pertanyaan di bawah ini
Berdasarkan diagram jaringan, layanan apa yang terekspos pada port TCP/10443?
VPN
Nah, server di balik IP 172.16.15.99 termasuk ke subnet yang mana?
Database subnet
Terakhir, apakah skenario tersebut tampak seperti True Positive (TP) atau False Positive (FP)?
TP

Teori Buku Latihan
Dengan inventaris aset dan diagram jaringan, Anda bisa mendapatkan konteks yang cukup tentang pengguna, host, atau alamat IP. Selanjutnya, tugas Anda adalah membuat penilaian apakah aktivitas yang Anda lihat aman atau tidak. Untuk beberapa peringatan, analisisnya sederhana, tetapi beberapa mungkin memerlukan puluhan langkah penting yang harus diikuti untuk menghindari hilangnya detail penting dan membingungkan bukti serangan. Jadi, bagaimana Anda dapat memastikan semua langkah analisis selalu diikuti?

Buku Kerja SOC
Buku kerja SOC , juga disebut playbook, runbook, atau alur kerja, adalah dokumen terstruktur yang mendefinisikan langkah-langkah yang diperlukan untuk menyelidiki dan mengatasi ancaman tertentu secara efisien dan konsisten. Karena analis L1 dianggap sebagai spesialis junior dan tidak diharapkan untuk melakukan triase setiap skenario serangan dengan sempurna, analis senior sering menyiapkan buku kerja untuk mendukung rekan tim mereka yang kurang berpengalaman. Analis L1 disarankan dan terkadang bahkan diharuskan untuk melakukan triase peringatan secara tepat sesuai dengan buku kerja untuk menghindari kesalahan dan memperlancar analisis.

Contoh Buku Kerja <img width="1342" height="690" alt="image" src="https://github.com/user-attachments/assets/f5ad1ec3-76db-4fbd-a6c9-816cac44457a" />
Diagram di atas adalah contoh tipikal buku kerja investigasi yang bertujuan untuk membantu analis L1 melakukan triase peringatan tentang email, web, atau login VPN perusahaan yang tidak lazim . Sebagian besar diagram buku kerja dilengkapi dengan panduan tekstual terperinci dan tautan ke sumber daya yang disebutkan. Perhatikan juga bagaimana buku kerja dibagi menjadi tiga kelompok logis. Dengan mengikuti langkah-langkah dalam urutan yang benar, Anda dapat menjamin triase peringatan berkualitas tinggi dan menghilangkan kasus di mana vonis dibuat tanpa bukti yang cukup:

Pengayaan : Gunakan intelijen ancaman dan inventaris identitas untuk mendapatkan informasi tentang pengguna yang terpengaruh.
Investigasi : Dengan menggunakan data yang terkumpul dan log SIEM , tentukan apakah login tersebut sesuai harapan.
Eskalasi : Eskalasikan peringatan ke L2 atau komunikasikan informasi login kepada pengguna jika diperlukan.
Jawablah pertanyaan-pertanyaan di bawah ini.

Peran SOC mana yang paling sering menggunakan buku kerja (misalnya Manajer SOC)?
SOC L1 Analyst

Bagaimana proses pengumpulan konteks pengguna, host, atau IP menggunakan TI dan pencarian?
Enrichment


Berdasarkan contoh buku kerja, platform apa yang digunakan sebagai sumber inventaris identitas?
BambooHR
Buku Latihan
Setiap tim memiliki pendekatan yang berbeda dalam membangun buku kerja. Beberapa tim mungkin memiliki ratusan buku kerja kompleks untuk setiap aturan deteksi yang mungkin, lebih mirip buku panduan otomatisasi SOAR daripada panduan manusia. Tim lain mungkin hanya menyiapkan beberapa buku kerja tingkat tinggi untuk vektor serangan yang paling umum dan lebih mengandalkan pengalaman dan pengambilan keputusan analis L1. Bagaimanapun, Anda sebagai analis L1 harus tahu cara membagi investigasi Anda menjadi blok-blok modular dan membangun buku kerja sederhana di sekitarnya.

Praktik
1.Ambil alih kepemilikan peringatan, gunakan peringatan inventaris identitas untuk mendapatkan konteks penerima email.
2.Selidiki email tersebut menggunakan EML Analyzer, termasuk reputasi SPF/DKIM, konten, dan domain pengirimnya.
3.Lanjutkan ke analisis lampiran, gunakan sandbox untuk biner dan kode manual, tinjau skrip.
4.Apakah lampiran tersebut berasal dari sumber berbahaya? Email yang dikirim melalui Orb tersebut tidak sesuai dengan peran penerima.
5.yes or not options
6.jika yes Kumpulkan bukti triase: daftar penerima, laporan sandbox, hasil analisis EML, indikator serangan lainnya dan Tulis laporan peringatan untuk L2 yang menjelaskan temuan dan melampirkan bukti triase ke laporan tersebut dan continue escalated to L2 / lanjutkan naik ke SOC L2 
7.jika not Tulis komentar singkat yang menjelaskan mengapa Anda yakin bahwa email tersebut aman dan sesuai harapan dan tutup peringatan

Kesimpulan
Kerja bagus dalam membangun buku kerja! Ingatlah untuk menggunakan pencarian yang sudah ada seperti inventaris aset atau peta jaringan untuk lebih memahami peringatan, dan dorong tim Anda untuk menerapkan dan memelihara buku kerja guna merampingkan dan menyederhanakan operasi SOC . Semoga Anda menikmati ruangan ini!



