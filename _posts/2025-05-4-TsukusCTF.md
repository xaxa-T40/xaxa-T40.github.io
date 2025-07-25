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



<!-- This theme supports **link posts**, made famous by John Gruber. To use, just add `link: http://url-you-want-linked` to the post's YAML front matter and you're done. -->

<!-- > And this is how a quote looks. -->

<!-- Some [link](#) can also be shown. -->