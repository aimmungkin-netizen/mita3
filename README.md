<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Filter Angka 4 Digit</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      padding: 20px;
    }

    .container {
      max-width: 900px;
      margin: auto;
      background: white;
      padding: 24px;
      border-radius: 14px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    h2 {
      margin-top: 0;
    }

    textarea {
      width: 100%;
      min-height: 160px;
      padding: 14px;
      border: 1px solid #ccc;
      border-radius: 10px;
      font-size: 16px;
      resize: vertical;
      box-sizing: border-box;
    }

    button {
      margin-top: 16px;
      margin-right: 10px;
      padding: 12px 24px;
      font-size: 16px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background: #111827;
      color: white;
    }

    button:hover {
      opacity: 0.9;
    }

    .result-box {
      margin-top: 28px;
    }

    .label {
      font-weight: bold;
      margin-bottom: 8px;
      display: block;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Filter Angka 4 Digit</h2>

    <label class="label">Input angka 4 digit (pisahkan dengan tanda *)</label>
    <textarea id="inputData" placeholder="Contoh: 1234*6862*8890*7765*7789"></textarea>

    <button onclick="prosesData()">Proses</button>

    <div class="result-box">
      <label class="label">Hasil Filter 1 (3/4 digit sama berurutan)</label>
      <textarea id="hasilData1" readonly placeholder="Hasil filter pertama..."></textarea>
      <button onclick="copyHasil('hasilData1', 'Hasil 1 berhasil disalin')">Copy Hasil 1</button>
    </div>

    <div class="result-box">
      <label class="label">Hasil Filter 2 (semua angka dengan 3/4 digit sama)</label>
      <textarea id="hasilData2" readonly placeholder="Hasil filter kedua..."></textarea>
      <button onclick="copyHasil('hasilData2', 'Hasil 2 berhasil disalin')">Copy Hasil 2</button>
    </div>
  </div>

  <script>
    // Filter 1: buang hanya jika 3 digit sama berurutan atau 4 digit sama
    function harusDibuangFilter1(angka) {
      if (!/^\d{4}$/.test(angka)) return false;

      // 4 digit sama: 1111, 7777
      if (/^(\d)\1\1\1$/.test(angka)) {
        return true;
      }

      // 3 digit sama berurutan: 8999, 1112, 7776
      if (/(\d)\1\1/.test(angka)) {
        return true;
      }

      // tetap pertahankan: 4454, 7787, 9979, 0900, 2232
      return false;
    }

    // Filter 2: buang semua angka yang memiliki 3 atau 4 digit sama
    function harusDibuangFilter2(angka) {
      if (!/^\d{4}$/.test(angka)) return false;

      const hitung = {};

      for (const digit of angka) {
        hitung[digit] = (hitung[digit] || 0) + 1;
      }

      const maksimum = Math.max(...Object.values(hitung));

      // buang jika ada digit muncul 3x atau 4x
      // contoh dibuang: 2232, 6676, 8898, 0900, 7777
      return maksimum >= 3;
    }

    function copyHasil(id, pesan) {
      const box = document.getElementById(id);
      box.select();
      box.setSelectionRange(0, 99999);

      navigator.clipboard.writeText(box.value)
        .then(() => {
          alert(pesan);
        })
        .catch(() => {
          alert('Gagal menyalin hasil');
        });
    }

    function prosesData() {
      const input = document.getElementById('inputData').value.trim();

      if (!input) {
        document.getElementById('hasilData1').value = '';
        document.getElementById('hasilData2').value = '';
        return;
      }

      const data = input
        .split('*')
        .map(item => item.trim())
        .filter(item => item !== '');

      const hasil1 = data.filter(item => !harusDibuangFilter1(item));
      const hasil2 = data.filter(item => !harusDibuangFilter2(item));

      document.getElementById('hasilData1').value = hasil1.join('*');
      document.getElementById('hasilData2').value = hasil2.join('*');
    }
  </script>
</body>
</html>
