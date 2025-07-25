---
title: "Tsukusu CTF"
categories:
  - Blog
tags:
  - link
  - Post Formats
---

### TSUKUSU CTF

**WEB**
**len-len (EASY)**
![alt text](/assets/images/Tsukusu/lenlen1.png)
1. Pada tantangan web ini kita dapat mendownlowd file zip nya, boleh download untuk lihat source codenya
2. coba aksess webnya
![alt text](/assets/images/Tsukusu/lenlen2.png)
3. Setelah dilihat kita harus memakai curl untuk mendapatkan flagnya 
4. lihat source docenya:
![alt text](/assets/images/Tsukusu/lenlen3.png)
5. Pada source codenya terlihat bagaimana cara kita harus mendapatkan flagnya yaitu dengan cara Panjang array harus kurang dari 0.      
![alt text](/assets/images/Tsukusu/lenlen4.png)
6. Pada perecobaan pertama saya ,mencoba mengisi array dengan angka 1-0, pada 
percobaaan pertama Panjang array tersebut lebih dari 10. Percobaan kedua hanya 
menyisakan kurung siku [] itu dihitung panjangnya = 2. Jadi kesimpulaanya setiap karakter dan angka setelah = itu dihitung,  Di dalam array dengan ‘[]’ kemungkinan tidak bisa membuat Panjang menjadi -1 . Jadi kita akan memakai tanda ‘{}’ untuk mendeskripsikan panjangnya.
![alt text](/assets/images/Tsukusu/lenlen5.png)
7. Tanda {} juga dihitung, berarti kitab isa langsung ke tahap exploit untuk 
mendeskripsikan panjangnya.
8. FLAG: `TsukuCTF25{l4n_lin_lun_l4n_l0n}`

**Flash (MEDIUM)**
![alt text](/assets/images/Tsukusu/flash1.png)
1. Tantangan web ini adalah dengan menjumlahkan angka setiap roundnya, yang jadi problem disini adalaha pada round ke 4-7, angkanya gak keluar, mari kita bedah, download flash zip dan coba akses webnya dan mulai
2. Tampilan Webnya  
![alt text](/assets/images/Tsukusu/flash2.png)
3. Masing masing round akan mengeluarkan angka acak dan round ke 4-7 angkanya tidak bisa dibaca
![alt text](/assets/images/Tsukusu/flash3.png)
4. Percobaan pertama masih salah, Bedah codenya:
![alt text](/assets/images/Tsukusu/flash4.png)
5. terdapat file seed.txt
![alt text](/assets/images/Tsukusu/flash5.png)
![alt text](/assets/images/Tsukusu/flash6.png)
6. Code tersebut adalah untuk membuat session setiap round yang akan menampilkan angkanya, jadi setiap round beda beda sessionnya.
![alt text](/assets/images/Tsukusu/flash7.png)
7. Code tersebut untuk supaya angka pada round ke 4 sampai 7 tidak terlihat dan menggunakan brupsuite untuk intercept apakah benar session id nya berubah-ubah
![alt text](/assets/images/Tsukusu/flash8.png)
8.  Mencoba intercept beberapa round dan benar semua session dalam round tersebut berbeda. Membuat exploitnya, logikanya: membuat program yang harus ada confignya ke chal ctf, input session per round, ketika roud itu visible maka akan mengambil file seed.txt dan akan menebak angka itu dari session dan seed.txt tersebut. dan jumlahkan hasilnya.
9. Code exploitnya: `TsukusuFlash/solve.py at main · justnitha/TsukusuFlash`
![alt text](/assets/images/Tsukusu/flash9.png)
10. Jumlahkan angka yang didapat dan hasilnya dan submit 
![alt text](/assets/images/Tsukusu/flash10.png)
11. Flag: `TsukuCTF25{Tr4d1on4l_P4th_trav3rs4l}` 


<!-- This theme supports **link posts**, made famous by John Gruber. To use, just add `link: http://url-you-want-linked` to the post's YAML front matter and you're done. -->

<!-- > And this is how a quote looks. -->

<!-- Some [link](#) can also be shown. -->