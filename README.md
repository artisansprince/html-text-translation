## Dokumentasi File: `Translate with Class Wrapper`

### Deskripsi
File ini adalah sebuah halaman web yang memungkinkan pengguna untuk menerjemahkan teks di dalam sebuah `div` berdasarkan pilihan bahasa. Menggunakan Google Translate API secara sederhana untuk menerjemahkan teks dalam elemen HTML yang terdapat di dalam `div` dengan kelas `translate-wrapper`. Pengguna dapat memilih bahasa yang diinginkan melalui dropdown yang tersedia, dan semua teks dalam `div` akan diterjemahkan ke bahasa yang dipilih.

### Struktur File
1. **Head**: 
   - Menyertakan metadata untuk karakter set dan viewport.
   - Menyertakan Tailwind CSS dari CDN untuk styling.
   
2. **Body**:
   - **Wrapper `div`**: Berisi elemen-elemen yang akan diterjemahkan, seperti heading, paragraf, dan list.
   - **Dropdown**: Sebuah dropdown (`<select>`) yang memungkinkan pengguna memilih bahasa yang diinginkan.

3. **Script**:
   - Terdapat dua fungsi utama di dalam `<script>`:
     - `translateText()`: Fungsi asinkron yang mengirimkan request ke Google Translate API untuk menerjemahkan teks.
     - `translateDivContent()`: Fungsi asinkron yang mencari dan menerjemahkan semua teks dalam elemen di dalam `div` dengan kelas tertentu.
   - Event listener pada dropdown untuk menangani perubahan bahasa dan menerjemahkan konten `div` yang sesuai.

---

### Penjelasan Komponen:

#### 1. **HTML Struktur**
```html
  <div class="translate-wrapper item-center">
    <h1 class="text-xl font-bold mb-2">Welcome to the translation demo!</h1>
    <p class="mb-2">This is a paragraph of text that will be translated.</p>
    <li>im the man</li>
    <li>im the boy</li>
    <p class="mb-2">Another paragraph here for testing.</p>
    <strong class="block mb-2">Important: This text will also be translated.</strong>
  </div>
```
- **`translate-wrapper`**: `div` pembungkus yang berisi berbagai elemen teks yang akan diterjemahkan.
- Elemen yang termasuk dalam wrapper ini dapat berupa heading (`h1`), paragraf (`p`), daftar (`li`), dan elemen `strong` yang dapat berisi teks tebal.
- Teks-teks di dalam elemen-elemen ini akan diterjemahkan ketika bahasa dipilih dari dropdown.

#### 2. **Dropdown untuk Pilihan Bahasa**
```html
  <select id="languageSelect" class="p-2 border border-gray-300 rounded-md shadow-md focus:outline-none focus:ring-2 focus:ring-blue-500">
    <option value="en">English</option>
    <option value="id">Indonesian</option>
    <option value="fr">French</option>
    <option value="es">Spanish</option>
  </select>
```
- Dropdown ini memungkinkan pengguna untuk memilih bahasa tujuan.
- Terdapat beberapa opsi bahasa: English (`en`), Indonesian (`id`), French (`fr`), dan Spanish (`es`).
- Saat pengguna memilih bahasa baru, konten dalam `div.translate-wrapper` akan diterjemahkan sesuai dengan bahasa yang dipilih.

#### 3. **JavaScript**
- **`translateText()`**: 
  Fungsi ini mengirimkan request ke Google Translate API dan menerima teks yang sudah diterjemahkan.
  
  ```javascript
  const translateText = async (text, targetLang) => {
    const url = `https://translate.googleapis.com/translate_a/single?client=gtx&sl=auto&tl=${targetLang}&dt=t&q=${encodeURIComponent(text)}`;
    const response = await fetch(url);
    const result = await response.json();
    return result[0][0][0];
  };
  ```

- **`translateDivContent()`**: 
  Fungsi ini mencari semua elemen di dalam `div` dengan kelas tertentu dan menerjemahkan teks di dalam elemen-elemen tersebut.
  
  ```javascript
  const translateDivContent = async (wrapperClass, targetLang) => {
    const wrapper = document.querySelector(`.${wrapperClass}`);
    if (wrapper) {
      const elements = wrapper.querySelectorAll("*"); // Ambil semua elemen dalam div
      for (const element of elements) {
        if (element.textContent.trim() !== "") { // Skip elemen kosong
          const translatedText = await translateText(element.textContent, targetLang);
          element.textContent = translatedText;
        }
      }
    }
  };
  ```

- **Event Listener untuk Dropdown**:
  Saat pilihan bahasa berubah, event listener akan memicu fungsi `translateDivContent()` untuk menerjemahkan seluruh teks di dalam `div.translate-wrapper` ke bahasa yang dipilih.

  ```javascript
  const languageSelect = document.getElementById("languageSelect");
  languageSelect.addEventListener("change", async () => {
    const selectedLanguage = languageSelect.value;
    await translateDivContent("translate-wrapper", selectedLanguage);
  });
  ```

---

### Cara Penggunaan:
1. Salin kode HTML ini ke dalam file dengan ekstensi `.html`.
2. Buka file di browser.
3. Pilih bahasa yang diinginkan dari dropdown untuk menerjemahkan teks yang ada di dalam div `translate-wrapper`.
4. Semua teks yang ada di dalam `div` akan diterjemahkan ke bahasa yang dipilih.

---

### Catatan:
- Halaman ini menggunakan Google Translate API untuk menerjemahkan teks. Penggunaan API ini mungkin memiliki batasan atau kuota tergantung pada penggunaan.
- Pastikan browser mendukung JavaScript agar fitur ini dapat berfungsi dengan baik.

---
