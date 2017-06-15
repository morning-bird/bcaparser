# KlikBCA Parser
Class ini berfungsi untuk login dan mengambil data daftar mutasi rekening di KlikBCA (m.klikbca.com)
Hasil modifikasi dari https://github.com/kadekjayak/bca-parser untuk keperluan pribadi

## Installation
Download class nya, kemudian include di project php mu...
	
	require 'BCAParser.php';

##Requirements
* PHP curl
* PHP openssl


## Example
fungsi kelas ini cuma 2 yaitu Login, dan mengambil tabel data transaksi berdasarkan range tanggal tertentu

### Login
ketika class ini di di buat, secara otomatis ia akan login ke klik bca melalui CURL

	$Parser = new BCAParser('username', 'password');
	
### Mengambil Mutasi Rekening
mengambil mutasi rekening dapat menggunakan method `getMutasiRekening` dengan parameter range tanggal transaksi yang diinginkan `getMutasiRekening(dari, sampai)`. Contoh :
	
	$Html = $Parser->getMutasiRekening('2016-08-1', '2016-08-12');

method ini me return element html `<table>` yang berisikan daftar transaksi

jika ingin hasil dalam format array gunakan method `getListTransaksi(dari, sampai)`, lihat source nya langsung.


### Mengambil Mutasi Rekening Credit
Untuk mengambil histori transaksi uang yang masuk ke dalam rekening (kredit) dapat menggunakan method `getTransaksiCredit` dengan parameter range tanggal transaksi yang diinginkan `getTransaksiCredit(dari, sampai)`. Contoh :
	
	$Html = $Parser->getTransaksiCredit('2016-11-20', '2016-11-27');

Struktur Array yang dihasilkan kurang lebih seperti berikut, perhatikan ada perbedaan value antara transaksi Antar Rekening dan transaksi Antar Bank :
	
	Array
	(
	    [3] => Array
	        (
	            [date] => 2016-11-22
	            [description] => TRSF E-BANKING CR  11/22 33223 Komentar Kadek Jayak 0000 20,000.00
	                )
                    [amount] => 20000.00
	            [flows] => CR
	        )

	    [4] => Array
	        (
	            [date] => 2016-11-25
	            [description] => SWITCHING CR TRANSFER  DR 013 Kadek Jayak Nama BANK 0999 200,000.00
                    [amount] => 200000.00
	            [flows] => CR
	        )

	)


### Logout
Logout dapat dilakukan dengan memanggil method `logout()`, pastikan anda logout setelah mengambil data transaksi, jika tidak kemungkinan anda harus menunggu 10 menit untuk dapat login melalui web KlikBca.


## Notes
Untuk debug atau menampilkan response CURL nya, cukup ubah value `BCA_PARSER_DEBUG` pada class ini menjadi `true`.
Aktivitas login dibatasi setiap 10 menit oleh bank, jika ingin membuat script "autocheck" pastikan waktu interval pengecekan nya diatas 10 menit !.

Update: 
- Lakukan Logout setelah mengambil data transaksi dari klikBca dengan begitu anda tidak perlu menunggu 10 menit untuk proses berikutnya *Perlu di Test
