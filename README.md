# store_sql
 database store menggunakan SQLite

> INVENTORY
	-id barang (primary key, tidak boleh kosong)
	-nama barang (tidak boleh kosong)
	-harga per barang (tidak boleh kosong)
	-banyak barang (default 0, otomatis keisi jika ada barang masuk dan keluar, banyak barang dihitung dari barang masuk - barang keluar(yang ada pada tabel transaksi barang))
	-ketersediaan barang (default 0(tidak tersedia), akan otomatis berubah menjadi 1(tersedia) jika banyak barang tidak sama dengan 0)

> HISTORY INVENTORY MASUK (harus mengisi data di tabel inventory terlebih dahulu)
	-id barang (foreign key ke tabel inventory)
	-id transaksi masuk (primary key, tidak boleh kosong)
	-banyak barang (tidak boleh kosong)
	-tanggal (tidak boleh kosong, default hari itu juga)

> HISTORY INVENTORY KELUAR (arus mengisi data di tabel inventory terlebih dahulu, akan otomatis keisi jika ada transaksi dilakukan)
	-id barang (foreign key ke tabel inventory)
	-id transaksi keluar (primary key, tidak boleh kosong)
	-banyak barang (tidak boleh kosong)
	-tanggal (tidak boleh kosong, default hari itu juga)
	-id transaksi (foreign key ke tabel transaksi, tidak boleh kosong)

> TRANSAKSI BARANG
	-id transaksi barang (primary key)
	-id transaksi (foreign key ke tabel transaksi)
	-nama barang (foreign key ke tabel inventory, menampilkan nama barang)
	-total barang (tidak boleh kosong)
	-total harga (total barang * harga per barang pada tabel inventory)

> TRANSAKSI (berisikan history transaksi)
	-id transaksi (primary key, tidak boleh kosong)
	-nama pelanggan (foreign key ke tabel pelanggan, tidak boleh kosong, menampilkan nama pelanggan)
	-nama karyawan (foreign key ke table karyawan, tidak boleh kosong, menampilkan nama karyawan)
	-id redeem (default null, foreign key ke tabel redeem, bisa ke isi lebih dari satu)
	-total harga sebelum di potong (total harga keseluruhan yang menggunakan id transaksi yang sama pada tabel transaksi barang)
	-total harga (total harga (diambil dari jumlah total harga keseluruhan pada tabel transaksi barang yang menggunakan id transaksi yang sama) - total redeem - total discount, tidak boleh kosong, default 0, perhitungan harga banyak barang atas id barang tersebut dikalikan dengan harga barang tersebut, tidak dapat digunakan jika melebihi limit voucher)
	-total harga redeem (default null, akan keisi harga jika id redeem keisi, jika keisi pada tabel voucer field id barang maka akan otomatis menambahkannya ke transaksi barang)
	-banyak barang (tidak boleh kosong, diambil dari total jumlah barang yang menggunakan id transaksi yang sama)
	-total discount (foreign key ke tabel dicount, akan memunculkan potongan harga)
	-total point (perhitungan point *total harga / 2000)
	-metode pembayaran (foreign key ke tabel metode pembayaran, akan menampilakan nama metode pembayaran, tidak boleh kosong, contoh: 1,tunai)
	-tanggal (tidak boleh kosong, default hari itu juga)

> KARYAWAN
	-id karyawan (primary key, tidak boleh kosong)
	-nama karyawan (tidak boleh kosong)
	-jabatan karyawan (tidak boleh kosong)
	-region bekerja (tidak boleh kosong, foreign ke tabel region, menampilkan nama region)
	-cabang bekerja (tidak boleh kosong, foreign ke tebel cabang, menampilkan nama cabang)
	-status (tidak boleh kosong, default 0(aktif), akan berubah menjadi 1 jika melawati masa aktif pada tabel informasi karyawan)

> INFORMASI KARYAWAN (harus mengisi data di tabel karyawan terlebih dahulu)
	-id karyawan (foreign key ke table karyawan,tidak boleh kosong)
	-nomor telepon (tidak boleh kosong)
	-email (default null)
	-alamat (tidak boleh kosong)
	-tanggal bergabung (tidak boleh kosong, terisi hari itu juga)
	-masa aktif (tidak boleh kosong)

> PELANGGAN
	-id pelanggan (primary key, tidak boleh kosong)
	-nama pelanggan (tidak boleh kosong)
	-tier (foreign key, yang akan keluar adalah nama tier, default perunggu)
	-banyak point (default 0, tidak boleh minus, point akan ke riset ke 0 jika melewati batas waktu expired, akan dikurang dengan total point yang diredeem)
	-waktu expired (tidak boleh kosong, akan terupdate menjadi bulan terakhir pada quarter tahun, contoh quarter pertama bulan 1 sampai 3 maka waktu expirednya akan menjadi bulan 3 dan setelah melewati bulan 3 maka akan terupdate menjadi bulan 6)
	-terkahir berbelanja (akan otomatis keisi tanggal terakhir melakukan transaksi, default null)	


> INFORMASI PELANGGAN
	-id pelanggan (primary key, tidak boleh kosong)
	-nomor telepon (default null)
	-email (default null)
	-alamat (default null)
	-tanggal bergabung (tidak boleh kosong, akan terisi hari itu juga)
	-total melakukan redeem (tidak boleh kosong, default 0, akan ke update jika telah berhasil melakukan redeem)

> TIER
	-id tier (primary key)
	-nama tier (tidak boleh kosong)
	-benefit tier
	-batas point (batas total harga transaksi untuk dapat bergabung di tier tersebut, jika melebihi batas point maka akan mengupdate data pada pelanggan ke tier selanjutnya)

> VOUCHER
	-id voucher (primary key, tidak boleh kosong)
	-id barang (foreign key ke inventory, default null)
	-nama voucher (tidak boleh kosong)
	-tipe voucher (foreign key ke tabel type voucher, tibak boleh kosong)
	-point yang dibutuhkan (tidak boleh kosong)
	-keterangan voucher (tidak boleh kosong)
	-potongan voucher (tidak boleh kosong)
	-tanggal mulai voucher (tidak boleh kosong)
	-tanggal berakhir voucher (tidak boleh kosong)
	-status voucher (tidak boleh kosong, default 1, akan berubah menjadi 0(tidak aktif) jika telah melebihi limit voucher yang di redeem)
	-maksimal redeem voucher (tidak boleh kosong)
	-limit penggunaan (tidak boleh kosong, default 1)

> TYPE VOUCHER
	-id voucher (primary key, tidak boleh kosong)
	-tipe voucher (tidak boleh kosong, contoh: 1-Barang Gratis, 2-Potongan harga)

> REDEEM
	-id redeem (primary key, tidak boleh kosong)
	-id voucher (foreign key ke tabel voucher, tidak boleh kosong)
	-id pelanggan (foreign key ke tabel pelanggan, tidak boleh kosong)
	-point yang dibutuhkan (diambil dari field point pada tabel voucher, tidak dapat di redeem jika point pelanggan lebih kecil dari total point yang dibutuhkan untuk di redeem, tidak boleh kosong, uniqe)
	-redeem code (akan kebentuk kombinasi antara angka dan huruf secara acak jika telah berhasil melakukan redeem, tidak boleh kosong)
	-tanggal redeem code (tidak boleh kosong, akan keisi jika berhasil melakukan redeem)
	-status redeem (default 1, tidak boleh kosong, status akan menjadi 0(tidak aktif) jika telah digunakan dalam transaksi)

> DISCOUNT
	-id discount (primary key, tidak boleh kosong)
	-id barang (foreign key ke tabel inventory, tidak boleh kosong)
	-nama discount (tidak boleh kosong
	-potongan harga (tidak boleh kosong)
	-tanggal mulai promo (tidak boleh kosong)
	-tanggal berakhir promo (tidak boleh kosong)

> METODE PEMBAYARAN
	-id pembayaran (primary key, tidak boleh kosong)
	-nama metode pembayaran (tidak boleh kosong, contoh: 1-tunai, 2-transfer)

> REGION
	-id region (primary key, tidak boleh kosong)
	-nama region (tidak boleh kosong)

> CABANG
	-id cabang (primary key, tidak boleh kosong)
	-nama region (foreign key ke tabel region dan menampilkan nama region)
	-nama cabang