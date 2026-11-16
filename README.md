# Prediksi Apakah Pengunjung Toko Online Akan Membeli

## Latar belakang dan tujuan
Sebagian orang yang berkunjung di toko onlie tidak akan belanja. data dataset kami yang berbelanja hanya sekitar 15,6%. ketika pemilik toko ingin memberikan promo terbatas pada orang orang tertentu yang memang ingin belanja saja dan tidak kehilangan pelanggan , maka toko bisa memperkirakan atau memprediksi, kira kira pelangganmana saja yang ingin membeli. Karena itu kami membangun model sederhana memprediksi pelanggan membeli atau tidak dengan melihat prilaku penggunjung tersebut.

Tujuan kami: kamui membuat model yang bisa memprediksi: apakah dalam pengunjung akan membeli atau tidak dengan cara melihat data perilaknya. Kami membandingkan dua model lalu membandingkan Logistic Regression dan Random Forest. Kami juga ingin tahu apa saja yang palin berpengaruh dalam tebakan. Kami tidak mengatakan model ini sudah siap untuk diguakan di kehidupannyata dan ini sebagai melatihan dalam membuat model ML.

## Dataset
dataset yang kami guakan  Adalah Dataset Online Shoppers Purchasing Intention dari UCI Machine Learning Repository (lisensi CC BY 4.0; makalah asli: Sakar dkk., 2019, Neural Computing and Applications). Didalanya ada  12.330 sesi kunjungan, 17 fitur dalam kolom dan ada 1 kolom yaitu  target Revenue .Tidak ada data kosong. Targetnya timpang: sekitar 84,5% tidak membeli dan 15,5% membeli. 
isi:
BounceRates: tentang berapa persen pengunjung data ke suatu halaman  kemudian keluar tanpa memencet menu di halaman itu.
•	ExitRates: tentang berapa persen suatu halaman menjadi halaman tehahir sebelum pengunjung  meningalkan situr toko
•	PageValues: nilai dari halaman yang dilihat pengguna sebelum transaksi selesai

## Jumlah data
| Tahap | Total | Tidak membeli | Membeli |
|---|---|---|---|
| Data mentah | 12.330 | 10.422 | 1.908 |
|Setelah hapus 125 baris kembar|	12.205 | 10.297	| 1.908 |
|Data latih (80%)	| 9.764	| 8.238	| 1.526 |
|Data ujian (20%) |	2.441	| 2.059	| 382 |

## Model
Logistic Regression (baseline). Model ini menghitung nilai dari semua fitur yang dikalikan bobot kemudian dijumlahkan jadi pelung pembelian. Kami memakainya karena sederhana dan cepat mudah dimengerti. Dipakai sebagai pembanding dasar namun membutuhkan diskalakan lebih dulu. 
Random Forest. Kumpulan banyak pertanyaan seperti pohon Keputusan (ya/tidak). Ia bisa melihat hubungan yang tidak lurus dan hubungan antar fitur, tidak perlu penskalaan, dan memberi ukuran kepentingan fitur. 

Data kami Adalah tabel berukuran sedang bisa ditakani ML, kami tidak memakai DL, karena berkemugkian menabah kerumitan dalam proses tanpa kebuhan pasti untuk kami.

Pengaturan yang kami pakai untuk keudannya Adalah Logistic Regression: class_weight='balanced', max_iter=1000. Random Forest: 200 pohon, kedalaman maksimal 10, class_weight='balanced'. Keduanya random_state=42

## Tahapan
### 1. Pemeriksaan dan pembersihan
dataset yang kami pakai memiliki 125 baris kembar(indentik di 18 kolom), kami tidak tahu apakah barus kembar ini bersal dari orang yang berbeda atau duplikat, semua kelompok baris kembar ini identik di semua kolom dan berada pada data yang berlebelkan tidak membeli. kami memutuskan untuk menghapus dan menyisakan satu dari baris kembar ini sebagai pola. menjadikan dataset yang tadinya 12.330 menjadi 12.205. kami menghapus baris baris kembar ini karena ketika sebagian data ada dalam data latih dan sebagian yag lain berada pada data ujian maka data kembar ini akan menjadi "kunci jawaban" dan membuat hasil pengujian akan terlalu optimis, maksudnya membuat nilai model lebih bagus dari kemampuan aslinya. harga yang harus kami bayar adaah membuat dataset kami kehilangan data tetang sesi yang kemungkinan besar adalah sesi "langsung keluar", walaupun proporsinya beda tipis(84,5% menjadi 84,4%) . kami melakukan permbersihan atau penghapusan baris data lewat kode jadi file CSV aslinya tidak kami ubah dan menjadikanya besifat sumber. Kami Talelah mengecek dan tidak menemuakn data kosong, sehingga tidak memerlukan langkah yang diperlukan Ketika data kosong.

### 2. Pemisahan target
kolom revenue kami pisah dari 17 kolom fitur lainnya, karena revenue bisa adalah "kunci jawaban" bagi model karena mengandung lebel "beli" dan "tidak beli".

### 3. Pengubahan kolom
kami mengubah data Month, OperatingSystems, Browser, Region, TrafficType, VisitorType, Weekend, yang seharusnya identitas ke one hot . untuk bagian seperti durasi jumlah halaman, kami tidak ubah dan hanya menyamakan skalanya. tujuannya untuk menjaga model tidak memberikan nilai pada indentitas. karena kode wilayah hanyalah identitas dan bukan nilai yang dihitung yang menjadikan seseorang itu membeli atau tidak. cotoh:w ilayah bernilai 7 tidak akan leblih besar nlainya dari wilayah 3 hanya karena  angkanya lebih besar, karena data identitas ini tidak bisa di perlakuakn sebagi bahan hitungan. Akibat dari kami menggunakan one hot adalah menjadikan kolomnya lebih banyak(58 kolom baru),karena setiap kolom identitas dipecah menjadi kolom kolom terpisah yang berskan angka 0 dan 1

### 4. Pembagian data
kami membagi dataset menjadi 80%(sekitar 9.764 baris) untuk latihan dan 20% untuk ujian(sekitar 2.441 baris total data ujian, dengan sekitar 382 dengan lebel "membeli"). alasan kami adalah karena data untuk ujian dan latihan haruslah data yang berbeda untuk menjaga data ujian tetap pada tujuannya yaitu menjadi alat ukur bagaimana kemungkinan kemampuan model, bagaimana kalau model dihadapkan dengan data baru di kehidupan nyata. jika data latihan dan ujian sama , maka hasil pegujian akan terlihat bagus hanya karena dapat mengingat dan tidak dapat melihat kemampuan model yang sebenarnya. dalam menentukan pembagian 20 persen untuk data ujian, kami memperkitakan data ujian 10% , 20%, 30% menghasilkan +- 6, +-4, +-3 point ketidak pastian dalam hasil : perkiraan untuk recall kelas “membeli” . 10% memberi kemungkinan hasil ujian cukup jauh(gep yang tinggi) , 20% memiliki nilai rentang yang tidak terlalu tinggi dan rendah, dan 30% memberikan nilai dengan rentang yang kecil, namun dengan data latih berkurang signifikan dengan manfaat yang semakin kecil, maka kami menggunakan komposisi 20% untuk data ujian yang seimbang.
dalam pembagian data kami lakukan dengan cara pemilihan "acak berlapis", maksudnya kami memilih pembagian data dengan acak dan tetap menjaga porsi  lebel "beli" dan "tidak membeli", membuat data ujian kami kemungkinan mencerminkan toko online, yang dimana sebagian besar orang tidak membeli(belum dipatikan). Pemilihan ini pula dipilih karena dataset kami tidak seimbang secara lebel karena dataset kami mendekati bagaimana kejadian di kehidupan nyata. kami menggunakan seed 42 dengan tujuan supaya hasil pembagian selalu sama saat kode dijalankan di percobaan. dalam pengujian ,data ujian tidak kami ubah dan tetap dengan proposi asli.

### 5. Penskalaan
Untuk 10 kolom numerik  memiliki rentang nilai yang berbeda dan Model logistic regression butuh nilai nilai kolom jumlah ini di skalakan supaya penilaian tidak berat sebelah, sedangkan untuk Random Forest tidak membutuhkannya. 

perhitungan rata rata dan sebaran dilakukan di latihan untuk menentukan skala dan dimana skala ini akan menjadi patokan saat pengukuran nilai dalam ujian.

### 6. Pelatihan
model hanya boleh belajar dari data latihan dan ujian hanya untuk mengukur. tiga hal yang hanya kami terapkan di data latih, yaitu: 
-pertama, kami melatih model mengenali dan mencari pola.
-kedua, perhitungan rata rata dan sebaran dilakukan di latihan untuk menentukan skala dan dimana skala ini akan menjadi patokan saat pengukuran nilai dalam ujian.
-ketiga, pembobotan kelas(class weight) digunakan dalam latihan supaya model lebih peka atau memperhatian kelompok beli, dan dalam ujian dibiarkan dengan pomposisi asli.
kami lakukan ini karena dalam dunia nyata pengunjugan online datang tidak berbarengan, sehingga ketika ujian model memiliki ukuran yang didapat dari data latihan tersebut.
data ujian tidak boleh di gunakan dalam latihan, niai ujian tidak dapat mencerminkan kemampuan asli dari model dan mebuat model seperti leblih hebat.

### 7. Evaluasi
model memprediksi data ujian, lalu diukur dengan akurasi, presisi, recall, F1, ROC-AUC, dan matriks kebingungan. kita tidak bisa hanya mengandalkan akurasi dalam melihat model itu bagus atau tidak, itu karena jumlah data dalam 2 katagori "membeli" dan tidak "tidak beli" datanya sangat timpang. jika sebuah model hanya mengatakan tidak , akan otomatis benar sekitar 84,4%, padahal model itu gagal menemukan orang yang benar benar membeli, maka dari itu diperlukan pengukuran lain untuk melihat "seberapa mampu model menemukan orang yang benar benar ingin membeli"

## Hasil
semua angka yang ada berassar dari data ujian yaitu 2441 sesi/baris dalam satu jali uji. jika model malas akurasinya akan 84,4% (2.059 dari 2.441) dengan recall 0(kemempuan menemuakan pembeli 0). kami menggunakan akurasi, presisi, recall, F1, ROC-AUC sebagai tamel pembanding. berdasarkan hasil matriks kebinungan .
Pada  Logistic Regression, ada 382 pembeli asli, model mnemukan 304 pembeli (recall 0,796) dan melewatkan 78 pembeli dengan  293 salah alarm. model Random Forest, kemampuan mereka sebenarnya hampir sama untuk menemukan pembeli asli 303 pembeli (recall 0,793) dan lewat 79 pembeli, dengan salam alarm(pengunjung yang tidak beli di kira beli) berkurang 64 salah alarm bedanya dengan Logistic.
Grafik Kurva ROC menampilkan model(Logistic Regression dan Random Forest) memiliki kemampuan yang sangat baik. dari hasil, kedua model memiliki kemampuan yang sangat baik untuk menemukan pembeli, selisih angka yang kecil dari keduanya ini dapat disesikan dengan keputusan bisnis.
## Kesimpulan
dalam pelatihan ini, perilaku pengunjung dapat memprediksi apakah pengunjung akan membeli atau tidak. Dalam kasus ini , Random Forest lebih cocok dimandingkan Logistic Regression, karena Random Forest lebih sedikit salah alarm. Namum medel ini masih masih sebagai latihan dan belum siap untuk digunakan di tokonyata, karna data terbatas pada satu toko dan sangat tergantung pada PageValues atau nilai dari jumlah rata rata halaman yang dibuka seseorang sebelum transaksi, dan perlunya mercobaan lebih lanjut.

## Referensi

- Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B., Grisel, O., Blondel, M., Prettenhofer, P., Weiss, R., Dubourg, V., Vanderplas, J., Passos, A., Cournapeau, D., Brucher, M., Perrot, M., & Duchesnay, É. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research*, *12*(85), 2825–2830. http://jmlr.org/papers/v12/pedregosa11a.html
- Sakar, C. & Kastro, Y. (2018). Online Shoppers Purchasing Intention Dataset [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5F88Q
- Sakar, C. O., Polat, S. O., Katircioglu, M., & Kastro, Y. (2019). Real-time prediction of online shoppers' purchasing intention using multilayer perceptron and LSTM recurrent neural networks. *Neural Computing and Applications*, *31*(10), 6893–6908. https://doi.org/10.1007/s00521-018-3523-0
