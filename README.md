<!DOCTYPE html>
<html lang="id" data-theme="dark">
<head>
  <meta charset="utf--8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kuis Pilihan Ganda A–E</title>
  <style>
    /* ====== THEME VARIABLES ====== */
    :root{
      /* dark */
      --bg:#0b1221; --ink:#e5e7eb; --muted:#94a3b8;
      --line:#1f2937;
      --cardTop:#0b1221; --cardBot:#0b1324;
      --panel:rgba(2,6,23,.9);
      --choice:#0f172a;
      --ok:#16a34a; --bad:#dc2626;
      --primary:#075985; --primaryBorder:#0891b2; --primaryInk:#e5e7eb;
      --badge:#334155;
      --gold:#d4af37;
    }
    html[data-theme="light"]{
      /* light dengan aksen emas */
      --bg:#f8fafc; --ink:#0b1221; --muted:#64748b;
      --line:#e2e8f0;
      --cardTop:#ffffff; --cardBot:#f7f8fa;
      --panel:rgba(255,255,255,.92);
      --choice:#ffffff;
      --ok:#15803d; --bad:#b91c1c;
      --primary:#d4af37; --primaryBorder:#d4af37; --primaryInk:#111827;
      --badge:#d4af37;
    }

    /* ====== BASE ====== */
    *{box-sizing:border-box}
    html,body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.5 "Inter", system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,"Helvetica Neue",Arial; scroll-padding-top: 8rem;}
    body{transition: background .2s, color .2s;}
    .wrap{max-width:980px;margin:auto;padding:24px}
    .header{display:flex;gap:12px;align-items:center;justify-content:space-between;margin-bottom:16px; flex-wrap:wrap;}
    .title{font-weight:700;font-size:clamp(18px,2.8vw,26px)}
    .legend{color:var(--muted);font-size:13.5px}
    .controls{display:flex;gap:8px;flex-wrap:wrap;align-items:center}

    .btn{background:#1f2937;color:#e5e7eb;border:1px solid var(--line);border-radius:12px;padding:10px 14px;cursor:pointer;transition:.2s}
    .btn:hover{transform:translateY(-1px); box-shadow: 0 4px 8px rgba(0,0,0,0.1);}
    .btn.primary{background:var(--primary);border-color:var(--primaryBorder);color:var(--primaryInk)}
    .btn.ghost{background:transparent}
    html[data-theme="light"] .btn{background:#f1f5f9;color:#0b1221;border-color:#e2e8f0}
    html[data-theme="light"] .btn:hover{background: #e2e8f0;}

    /* UPDATED: Select styling for better structure */
    .select-wrapper {
      position: relative;
      display: inline-block;
      vertical-align: middle;
    }
    .select-wrapper::after {
      content: "▾";
      position: absolute;
      right: 14px;
      top: 50%;
      transform: translateY(-50%);
      pointer-events: none;
      color: var(--muted);
    }
    .select-wrapper select {
      appearance: none;
      -webkit-appearance: none;
      background: #1f2937;
      color: var(--ink);
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 10px 40px 10px 14px;
      cursor: pointer;
      outline: none;
      font: inherit;
      transition: border-color .2s;
    }
    .select-wrapper select:hover {
        border-color: #3b82f6;
    }
    html[data-theme="light"] .select-wrapper select {
      background: #f1f5f9;
      color: #0b1221;
      border-color: #e2e8f0;
    }

    .label{font-size:13px;color:var(--muted);margin-right:6px}

    .mode-badge{border:1px solid var(--badge); color:var(--muted); padding:2px 8px; border-radius:999px; font-size:12px}

    .result{position:sticky;top:16px;background:var(--panel);backdrop-filter:blur(8px);border:1px solid var(--line);border-radius:12px;padding:16px;margin:24px 0; z-index: 10;}
    .bar{height:10px;background:var(--line);border-radius:999px;overflow:hidden;margin-top:8px}
    .bar>span{display:block;height:100%;background:linear-gradient(90deg,#22c55e,#16a34a);width:0%; transition: width .5s ease-out;}

    .card{background:linear-gradient(180deg,var(--cardTop) 0%,var(--cardBot) 100%);border:1px solid var(--line);border-radius:16px;padding:20px;margin:16px 0; transition: border .2s;}
    .qhead{display:flex;align-items:baseline;gap:10px;margin-bottom:8px}
    .qno{font-weight:700;color:#93c5fd}
    html[data-theme="light"] .qno{color:#1d4ed8}
    .qtext{font-weight:600}
    fieldset{border:0;margin:0;padding:0}
    .choices{display:grid;gap:8px;margin-top:12px}
    .choice{display:flex;align-items:flex-start;gap:10px;background:var(--choice);border:1px solid var(--line);border-radius:12px;padding:12px;cursor:pointer; transition: border-color .2s, background-color .2s;}
    .choice:hover{border-color:#3b82f6}
    html[data-theme="light"] .choice:hover{border-color:#3b82f6}
    .choice input{flex-shrink:0; margin-top:4px}
    .badge{display:inline-block;font-size:12px;padding:2px 8px;border-radius:999px;border:1px solid var(--badge);color:var(--muted); margin-right: 4px;}
    html[data-theme="light"] .badge{border-color:var(--gold);color:#8a6b00}
    
    /* Status styling after grading */
    .correct-answer-label { border-color: var(--ok) !important; background-color: rgba(22, 163, 74, 0.1);}
    .wrong-answer-label { border-color: var(--bad) !important; background-color: rgba(220, 38, 38, 0.1);}
    .correct .qhead { color: var(--ok); }
    .wrong .qhead { color: var(--bad); }

    .summary{background:linear-gradient(180deg,var(--cardTop) 0%,var(--cardBot) 100%);border:1px solid var(--line);border-radius:16px;padding:18px;margin:24px 0}
    .sumhead{display:flex;gap:12px;flex-wrap:wrap;color:#cbd5e1; align-items: center; margin-bottom: 8px;}
    html[data-theme="light"] .sumhead{color:#334155}
    .sumtable{width:100%;border-collapse:collapse;margin-top:10px;font-size:14px}
    .sumtable th,.sumtable td{border-top:1px solid var(--line);padding:8px 6px;text-align:left;vertical-align:top}
    .status-ok{color:var(--ok);font-weight:700}
    .status-bad{color:var(--bad);font-weight:700}
    
    .explanation-row td { border-top: none; padding-top: 0; padding-bottom: 12px; font-size: 13px; color: var(--muted); }
    html[data-theme="light"] .explanation-row { background: #f8fafc; }
    html[data-theme="dark"] .explanation-row { background: #111827; }
    .explanation-cell strong { color: var(--ink); }

    .foot{margin-top:24px;color:var(--muted);font-size:13px; text-align: center;}

    /* NEW: Styling for the bottom grade button container */
    .grade-container {
      margin: 32px 0 24px 0;
      text-align: center;
    }
    .grade-container .btn {
      padding: 12px 32px;
      font-size: 1.1rem;
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header class="header">
      <div>
        <h1 class="title">Kuis Pilihan Ganda A–E</h1>
        <p class="legend">Pilih satu jawaban setiap nomor. Jawaban tersimpan otomatis di browser. <span class="mode-badge">Light mode punya aksen emas</span></p>
      </div>
      <div class="controls">
        <label for="limitSelect" class="label">Jumlah Soal:</label>
        <!-- UPDATED: Refactored select element for cleaner code -->
        <div class="select-wrapper">
          <select id="limitSelect">
            <option>10</option>
            <option>20</option>
            <option>30</option>
            <option>40</option>
            <option>50</option>
            <option>60</option>
            <option>70</option>
            <option>80</option>
            <option>90</option>
            <option>100</option>
            <option>110</option>
            <option>120</option>
            <option>130</option>
            <option>140</option>
            <option>151</option>
          </select>
        </div>

        <button class="btn ghost" id="modeToggle" title="Ganti tema (Dark/Light)">🌙 / ☀️</button>
        <button class="btn ghost" id="shuffleQuestions" title="Acak urutan soal">Acak Soal</button>
        <button class="btn ghost" id="shuffleOptions" title="Toggle pengacakan opsi">Acak Opsi: <span id="optState">ON</span></button>
        <button class="btn" id="reset">Reset</button>
        <!-- MOVED: The grade button is no longer here -->
      </div>
    </header>

    <div class="result" aria-live="polite">
      <div>
        <strong>Skor:</strong> <span id="score">0</span>/<span id="total">0</span>
        (<span id="percent">0</span>%) •
        <strong>Poin:</strong> <span id="points">0</span>
      </div>
      <div class="bar" aria-hidden="true"><span id="barfill"></span></div>
    </div>

    <form id="quiz" novalidate></form>

    <!-- NEW: Button is now placed here, after the questions -->
    <div class="grade-container">
      <button class="btn primary" id="grade">Koreksi Jawaban</button>
    </div>

    <section id="summary" class="summary" hidden>
      <div class="sumhead">
        <h2 style="font-size: 1.2em; margin:0;">Rekap Hasil</h2>
        <div>| Terjawab: <span id="sum-answered">0</span></div>
        <div>| Benar: <span id="sum-correct">0</span></div>
        <div>| Poin: <span id="sum-points">0</span></div>
      </div>
      <table class="sumtable" id="sum-table">
        <thead>
          <tr>
            <th style="width:48px">No</th>
            <th>Pertanyaan</th>
            <th style="width:210px">Jawaban Anda</th>
            <th style="width:210px">Kunci</th>
            <th style="width:90px">Status</th>
            <th style="width:70px">Poin</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </section>

    <footer class="foot">
      Edit/cek data di konstanta <code>DATA</code> di JavaScript. Format: <code>Soal|A|B|C|D|E|kunci|Pembahasan (opsional)</code>.
    </footer>
  </div>

  <script>
    /* ================== DATA SOAL (UPDATED with full explanations) ================== */
    const DATA = `
Abolisi artinya …|penegakan hukum|pengakuan|penghapusan, pembatalan|pelestarian|penghargaan|c|Menurut KBBI, abolisi adalah penghapusan peristiwa pidana.
Abonemen artinya …|berlangganan|penghentian|penolakan|pemutusan kontrak|pelepasan|a|Abonemen berasal dari bahasa Belanda 'abonnement' yang berarti berlangganan.
Absen artinya …|tidak masuk, tidak hadir|hadir|aktif|ikut serta|berpartisipasi|a|Kata 'absen' sering disalahartikan sebagai 'mengisi daftar hadir', padahal artinya adalah tidak hadir.
Absolut artinya …|mutlak, tidak terbatas|relatif|terbatas|nisbi|kondisional|a|Absolut berarti mutlak, tidak terikat oleh batasan atau syarat apa pun.
Absorpsi artinya …|penyerapan|pengeluaran|pelepasan|pembuangan|pengusiran|a|Absorpsi adalah proses penyerapan suatu zat oleh zat lain, misalnya penyerapan air oleh spons.
Afeksi artinya …|kasih sayang|kebencian|dendam|permusuhan|sikap acuh|a|Afeksi adalah perasaan kasih sayang atau kehangatan emosional terhadap orang lain.
Afinitas artinya …|ketertarikan, simpati|penolakan|antipati|permusuhan|kebencian|a|Afinitas adalah daya tarik atau rasa suka (simpati) terhadap sesuatu atau seseorang.
Agitasi artinya …|hasutan|ajakan damai|ketenangan|penenangan|menenangkan|a|Agitasi adalah tindakan menghasut sekelompok orang untuk menimbulkan kekacauan atau pemberontakan.
Agraria artinya …|urusan pertanian|industri berat|perdagangan|teknologi|perkapalan|a|Agraria adalah segala urusan yang berkaitan dengan pertanian atau kepemilikan tanah.
Aberasi artinya …|menyimpang jauh dari tujuan|fokus|lurus|konsisten|tepat sasaran|a|Aberasi adalah penyimpangan dari keadaan yang normal atau yang benar.
Akselerasi artinya …|percepatan|perlambatan|penundaan|keterlambatan|penghambatan|a|Akselerasi adalah laju perubahan kecepatan, atau proses percepatan suatu gerakan atau kegiatan.
Aktual artinya …|nyata, hangat|khayalan|fiktif|ilusi|bayangan|a|Aktual berarti sesuatu yang benar-benar terjadi, sedang menjadi pembicaraan, atau nyata.
Akurasi artinya …|ketelitian, kecermatan|ceroboh|sembrono|asal-asalan|teledor|a|Akurasi adalah tingkat ketelitian atau kecermatan yang mendekati nilai sebenarnya.
Akurat artinya …|teliti, seksama, cermat|serampangan|ngawur|asal-asalan|sembarangan|a|Akurat adalah sifat teliti, cermat, dan tidak menyimpang dari sasaran.
Aliansi artinya …|persekutuan, ikatan|permusuhan|pertentangan|konflik|perselisihan|a|Aliansi adalah ikatan atau persekutuan antara dua pihak atau lebih (bisa negara, kelompok) untuk tujuan bersama.
Ambigu artinya …|bermakna lebih dari satu|jelas|pasti|tegas|terang|a|Ambigu adalah sifat sesuatu (kalimat atau kata) yang memiliki makna ganda atau lebih dari satu.
Anonim artinya …|tanpa nama, tak beridentitas|terkenal|populer|tersohor|identitas jelas|a|Anonim berarti tidak memiliki nama atau identitas yang jelas.
Apatis artinya …|tidak peduli, acuh tak acuh|peduli|perhatian|empati|aktif|a|Apatis adalah sikap acuh tak acuh atau tidak peduli terhadap lingkungan sekitar.
Arbitrer artinya …|sewenang-wenang|adil|bijaksana|netral|berimbang|a|Arbitrer adalah tindakan atau keputusan yang diambil secara sewenang-wenang tanpa berdasarkan aturan yang jelas.
Aristokrat artinya …|bangsawan, ningrat|rakyat jelata|orang biasa|kaum buruh|masyarakat umum|a|Aristokrat merujuk pada kaum bangsawan atau kalangan atas dalam tatanan sosial.
Arogan artinya …|congkak, sombong, angkuh|rendah hati|sederhana|sopan|tawaduk|a|Arogan adalah sifat sombong, congkak, dan merasa lebih unggul dari orang lain.
Artifisial artinya …|buatan, tidak alami|alami|asli|murni|organik|a|Artifisial berarti buatan atau tidak alami, seperti pada istilah 'kecerdasan artifisial'.
Blangko artinya …|kosong|terisi|penuh|padat|lengkap|a|Blangko (atau blanco) berarti kosong, seperti pada formulir yang belum diisi.
Candu artinya …|adiktif|menenangkan|obat sehat|penyembuhan|penawar|a|Candu adalah sesuatu yang dapat menyebabkan ketergantungan atau adiksi bagi penggunanya.
Definit artinya …|tertentu, pasti|samar|ambigu|meragukan|tidak jelas|a|Definit berarti sudah pasti dan tidak dapat diubah lagi; tertentu.
Defisit artinya …|kekurangan|kelebihan|surplus|berlimpah|berlebih|a|Defisit adalah kondisi kekurangan, terutama dalam hal keuangan (anggaran belanja lebih besar dari pendapatan).
Degradasi artinya …|kemerosotan, kemunduran|peningkatan|kemajuan|perkembangan|perbaikan|a|Degradasi adalah proses kemerosotan atau kemunduran mutu, pangkat, atau moral.
Delik artinya …|tindak pidana|kebaikan|perbuatan baik|pahala|amal|a|Delik adalah istilah dalam hukum yang berarti perbuatan yang dapat dikenai hukuman karena merupakan pelanggaran undang-undang; tindak pidana.
Delusi artinya …|khayal|kenyataan|fakta|realitas|benar|a|Delusi adalah keyakinan atau pandangan yang salah (berdasarkan khayal) yang dipertahankan meskipun ada bukti sebaliknya.
Dependensi artinya …|ketergantungan|kemandirian|otonomi|berdiri sendiri|mandiri|a|Dependensi adalah keadaan bergantung pada orang lain atau sesuatu yang lain.
Depresi artinya …|stagnasi|perkembangan|kemajuan|pertumbuhan|ekspansi|a|Dalam konteks ekonomi, depresi adalah keadaan stagnasi atau kemacetan yang parah dan berkepanjangan.
Desalinasi artinya …|penyulingan, proses membuat air laut|pengotoran|pencemaran|penambahan garam|penggaraman|a|Desalinasi adalah proses menghilangkan kadar garam dari air (misalnya air laut) agar menjadi air tawar.
Deskripsi artinya …|pemaparan, penggambaran|penyembunyian|pengaburan|penghilangan|penyamaran|a|Deskripsi adalah pemaparan atau penggambaran suatu objek atau keadaan dengan kata-kata secara jelas.
Destruktif artinya …|merusak, menghancurkan|membangun|memperbaiki|melestarikan|memperindah|a|Destruktif adalah sifat yang cenderung merusak atau menghancurkan.
Dinamis artinya …|penuh semangat|pasif|malas|lesu|statis|a|Dinamis berarti penuh semangat, mudah berubah, dan terus bergerak mengikuti perkembangan.
Doktrin artinya …|ajaran|kebingungan|kebodohan|ketidaktahuan|penyimpangan|a|Doktrin adalah ajaran atau seperangkat kepercayaan (dalam agama, politik) yang diyakini kebenarannya.
Duplikat artinya …|replika, salinan, tiruan|asli|otentik|murni|orisinal|a|Duplikat adalah salinan atau tiruan yang sama persis dengan aslinya.
Efesien artinya …|berdaya guna, tepat guna|boros|mubazir|sia-sia|tidak efektif|a|Efisien (bentuk baku: efisien) berarti tepat atau sesuai untuk menghasilkan sesuatu (berdaya guna, tepat guna).
Eksemplar artinya …|lembar, helai|buku utuh|jilid|kumpulan|volume|a|Eksemplar adalah satuan untuk benda-benda terbitan seperti buku, majalah, atau koran; lembar.
Eksepsi artinya …|pengecualian|kebiasaan|kelaziman|keumuman|aturan umum|a|Eksepsi adalah pengecualian dari aturan umum yang berlaku.
Eksodus artinya …|hijrah, mengungsi|menetap|tinggal|menetapkan diri|berdiam|a|Eksodus adalah perpindahan penduduk secara besar-besaran dari satu daerah ke daerah lain.
Ekspansi artinya …|perluasan wilayah|penyempitan|pengurangan|penyusutan|pengecilan|a|Ekspansi adalah usaha untuk memperluas wilayah kekuasaan, daerah pemasaran, atau kegiatan usaha.
Ekspedisi artinya …|pengiriman surat atau barang|penerimaan|penerima|pembatalan|penolakan|a|Ekspedisi adalah kegiatan pengiriman surat atau barang, atau bisa juga berarti perjalanan penyelidikan ilmiah.
Eksploitasi artinya …|pemanfaatan, pendayagunaan|penyia-nyiaan|pembiaran|pengabaian|pembuangan|a|Eksploitasi adalah pemanfaatan sumber daya untuk keuntungan sendiri, seringkali berkonotasi negatif (pemanfaatan berlebihan).
Eksplorasi artinya …|pendalaman, penggalian, pengkajian|pengabaian|penutupan|penghentian|menutup akses|a|Eksplorasi adalah kegiatan penjelajahan atau penggalian informasi untuk tujuan penemuan atau penelitian.
Ekspresi artinya …|ungkapan, pandangan air muka|menahan diri|menyembunyikan|menyamarkan|memendam|a|Ekspresi adalah pengungkapan maksud, gagasan, atau perasaan, seringkali melalui raut muka atau kata-kata.
Ekstra artinya …|tambahan, sangat luar biasa|biasa|normal|standar|wajar|a|Ekstra berarti tambahan di luar yang biasa, atau bisa juga berarti sangat luar biasa.
Ekuilibrium artinya …|kesetimbangan|ketimpangan|ketidakseimbangan|kekacauan|ketidakteraturan|a|Ekuilibrium adalah keadaan setimbang atau seimbang antara kekuatan-kekuatan yang berlawanan.
Ekuivalen artinya …|sama, sebanding|berbeda|tidak seimbang|tidak setara|timpang|a|Ekuivalen berarti memiliki nilai, ukuran, atau arti yang sama; sebanding.
Elusif artinya …|sulit dipahami atau diartikan|jelas|gamblang|tegas|terang|a|Elusif adalah sifat sesuatu yang sulit untuk dipahami, ditangkap, atau diartikan dengan jelas.
Embargo artinya …|larangan|kebebasan|izin|kelonggaran|persetujuan|a|Embargo adalah larangan yang dikeluarkan pemerintah terhadap perdagangan dengan negara lain.
Entitas artinya …|satuan yang berwujud|tidak nyata|abstrak|semu|khayalan|a|Entitas adalah satuan yang memiliki wujud tersendiri dan dapat dibedakan dari yang lain.
Enumerasi artinya …|pencacahan, penjumlahan|pengurangan|penghapusan|penghilangan|pemangkasan|a|Enumerasi adalah pencacahan atau penghitungan satu per satu.
Esensi artinya …|hakikat|kepalsuan|ilusi|permukaan|kulit luar|a|Esensi adalah hakikat, inti, atau hal yang paling pokok dari sesuatu.
Epilog artinya …|penutup|pembuka|pendahuluan|awal|pengantar|a|Epilog adalah bagian penutup pada karya sastra yang fungsinya untuk menyampaikan kesimpulan atau nasib akhir para tokoh.
Eskalasi artinya …|kenaikan|penurunan|pengurangan|penurunan drastis|kemunduran|a|Eskalasi adalah kenaikan atau peningkatan (misalnya, eskalasi konflik berarti konflik yang semakin memanas).
Estetis artinya …|keindahan|keburukan|kekacauan|ketidakharmonisan|kerusakan|a|Estetis adalah segala sesuatu yang berkaitan dengan keindahan atau seni.
Estimasi artinya …|perkiraan|kepastian|ketetapan|kejelasan|fakta pasti|a|Estimasi adalah perkiraan atau penilaian awal terhadap sesuatu.
Estuari artinya …|muara|gunung|bukit|daratan|tebing|a|Estuari adalah badan air di pesisir tempat bertemunya air tawar dari sungai dengan air asin dari laut; muara.
Etika artinya …|akhlak|kebiadaban|ketidaksopanan|perilaku buruk|kejahatan|a|Etika adalah ilmu tentang apa yang baik dan buruk mengenai hak dan kewajiban moral (akhlak).
Evakuasi artinya …|pemindahan|penahanan|pengurungan|pengurungan diri|pemasungan|a|Evakuasi adalah proses pemindahan manusia atau barang dari daerah berbahaya ke tempat yang lebih aman.
Etnis artinya …|etnik|internasional|global|umum|universal|a|Etnis (atau etnik) berkaitan dengan kelompok sosial yang didasarkan pada kesamaan keturunan, adat, bahasa, dsb; suku bangsa.
Evaluasi artinya …|penilaian|pengabaian|penghindaran|pembiaran|pengacuhan|a|Evaluasi adalah proses pemberian nilai atau penilaian terhadap kinerja atau hasil sesuatu.
Evaporasi artinya …|penguapan|pembekuan|pengendapan|pengkristalan|pemadatan|a|Evaporasi adalah proses perubahan zat cair menjadi gas atau uap; penguapan.
Evokasi artinya …|daya penggugah rasa|penghilangan rasa|pemadaman emosi|penekanan|penghapusan kesan|a|Evokasi adalah kemampuan untuk membangkitkan atau menggugah emosi, kenangan, atau perasaan.
Evolusi artinya …|perubahan|kemunduran|stagnasi|berhenti|tetap|a|Evolusi adalah perubahan (pertumbuhan, perkembangan) secara berangsur-angsur dan perlahan-lahan.
Faksi artinya …|kelompok|individu|tunggal|sendiri|pribadi|a|Faksi adalah kelompok di dalam suatu partai politik atau organisasi yang memiliki pandangan berbeda dari kelompok lain.
Faktor artinya …|penyebab|akibat|hasil|dampak|konsekuensi|a|Faktor adalah hal atau keadaan yang ikut menyebabkan terjadinya sesuatu.
Faktual artinya …|berdasarkan kenyataan|fiktif|khayalan|ilusi|rekaan|a|Faktual berarti sesuatu yang didasarkan pada kenyataan atau mengandung fakta.
Fana artinya …|tidak kekal|abadi|kekal|selamanya|lestari|a|Fana berarti tidak kekal atau akan rusak pada waktunya.
Fatamorgana artinya …|khayal|nyata|asli|wujud|fakta|a|Fatamorgana adalah ilusi optik yang tampak seperti genangan air di padang pasir atau jalan raya; bersifat khayal.
Fenomena artinya …|gejala|kepastian|ketetapan|hukum mutlak|kebenaran pasti|a|Fenomena adalah hal atau gejala yang dapat disaksikan dengan pancaindra dan dapat diterangkan secara ilmiah.
Feodal artinya …|aristokrat|rakyat biasa|petani jelata|orang kecil|rakyat jelata|a|Feodal adalah sistem sosial yang mengagung-agungkan jabatan atau pangkat dan bukan prestasi kerja; berkaitan dengan kaum aristokrat.
Fermentasi artinya …|peragian|pembusukan|pembakaran|pemanasan|pendinginan|a|Fermentasi adalah proses penguraian zat organik oleh mikroorganisme yang menghasilkan energi; peragian.
Fiksi artinya …|khayalan, rekaan|nyata|fakta|kebenaran|realitas|a|Fiksi adalah cerita rekaan atau kisahan yang berdasarkan khayalan dan bukan kenyataan.
Fiktif artinya …|bersifat fiksi|nyata|asli|benar|faktual|a|Fiktif adalah sifat sesuatu yang hanya terdapat dalam khayalan; bersifat fiksi.
Fiskal artinya …|perpajakan|hukum pidana|hukum perdata|tata negara|filsafat|a|Fiskal adalah segala sesuatu yang berkaitan dengan urusan pajak atau pendapatan negara.
Fluktuatif artinya …|bersifat fluktuasi|tetap|stabil|konstan|tidak berubah|a|Fluktuatif adalah keadaan yang menunjukkan perubahan (naik turun) secara tidak teratur; bersifat fluktuasi.
Friksi artinya …|perpecahan|persatuan|kesatuan|kebersamaan|kerukunan|a|Friksi adalah pergeseran yang menimbulkan perpecahan atau pertentangan antarpihak.
Fusi artinya …|penggabungan|pemisahan|pemecahan|perpecahan|perbedaan|a|Fusi adalah proses penggabungan dua atau lebih entitas menjadi satu.
Gema artinya …|suara yang memantul|keheningan|kesunyian|hening|diam|a|Gema adalah suara yang terdengar kembali karena pantulan.
Grasi artinya …|pengampunan|penghukuman|penjatuhan pidana|penahanan|pemenjaraan|a|Grasi adalah pengampunan hukuman yang diberikan oleh kepala negara kepada terpidana.
Harmoni artinya …|keselarasan, keserasian|kekacauan|keributan|ketidakteraturan|disharmoni|a|Harmoni adalah keadaan selaras atau serasi, misalnya dalam musik atau hubungan sosial.
Heksagonal artinya …|segienam|segitiga|segilima|segiempat|segidelapan|a|Heksagonal adalah bentuk yang memiliki enam sisi dan enam sudut; segi enam.
Ideologi artinya …|paham, teori|kebingungan|ketidaktahuan|tanpa konsep|hampa|a|Ideologi adalah kumpulan konsep atau paham yang menjadi dasar pendapat atau tujuan.
Ikhtiar artinya …|daya, upaya|menyerah|pasrah|malas|berhenti|a|Ikhtiar adalah usaha atau daya upaya untuk mencapai suatu maksud atau tujuan.
Ikhtisar artinya …|ringkasan|uraian panjang|penjelasan detail|cerita lengkap|pembahasan luas|a|Ikhtisar adalah pandangan secara ringkas atau ringkasan dari suatu uraian.
Imitasi artinya …|tiruan, bukan asli|asli|otentik|murni|orisinal|a|Imitasi adalah barang tiruan yang dibuat menyerupai aslinya.
Implikasi artinya …|keterlibatan|pelepasan|pembebasan|penolakan|pengasingan|a|Implikasi adalah akibat atau keterlibatan yang timbul dari suatu kejadian atau keputusan.
Impuls artinya …|dorongan hati|pertimbangan matang|perhitungan|kalkulasi|rencana|a|Impuls adalah dorongan hati yang timbul secara tiba-tiba untuk melakukan sesuatu tanpa pertimbangan.
Imun artinya …|kebal|rentan|lemah|mudah sakit|terjangkit|a|Imun berarti kebal atau tahan terhadap penyakit.
Individualis artinya …|egois|sosial|kolektif|kebersamaan|peduli|a|Individualis adalah orang yang mementingkan diri sendiri atau bersikap egois.
Inferensi artinya …|kesimpulan|pertanyaan|keraguan|kebingungan|praduga|a|Inferensi adalah proses penarikan kesimpulan berdasarkan data atau fakta yang ada.
Informal artinya …|tidak resmi|resmi|formal|sah|legal|a|Informal berarti tidak resmi atau santai.
Inisiasi artinya …|upacara, meresmikan|pembatalan|penolakan|penutupan|penghentian|a|Inisiasi adalah upacara atau proses peresmian seseorang masuk ke dalam suatu kelompok atau tingkatan.
Inklusif artinya …|termasuk, terhitung|mengecualikan|membatasi|menyingkirkan|mengeluarkan|a|Inklusif berarti mencakup atau termasuk di dalamnya, tidak membatasi atau mengecualikan.
Inovasi artinya …|pembaharuan|kebiasaan lama|tradisi usang|cara lama|stagnasi|a|Inovasi adalah penemuan atau pengenalan hal-hal baru; pembaharuan.
Inses artinya …|perkawinan antara dua orang yang bersaudara dekat|pernikahan sah|pernikahan halal|hubungan resmi|ikatan sah|a|Inses adalah hubungan seksual atau perkawinan antara orang-orang yang memiliki hubungan keluarga dekat yang terlarang oleh hukum atau adat.
Inspeksi artinya …|pemeriksaan|pembiaran|pengabaian|melalaikan|melewatkan|a|Inspeksi adalah pemeriksaan secara saksama untuk mengetahui keadaan sebenarnya.
Inspirasi artinya …|ilham|keputusasaan|kehilangan ide|kebuntuan|putus asa|a|Inspirasi adalah ilham atau percikan ide kreatif yang timbul dalam pikiran.
Instruksi artinya …|pelajaran, petunjuk|kebingungan|larangan|teguran|ancaman|a|Instruksi adalah perintah atau petunjuk untuk melakukan sesuatu.
Integrasi artinya …|pembauran|pemisahan|pengasingan|perpecahan|keterpisahan|a|Integrasi adalah proses pembauran atau penyatuan berbagai kelompok hingga menjadi satu kesatuan yang utuh.
Intelektual artinya …|cendekiawan, cerdas|bodoh|awam|dungu|tidak terpelajar|a|Intelektual merujuk pada kaum terpelajar, cerdas, atau cendekiawan.
Interogasi artinya …|pemeriksaan, pertanyaan|pembiaran|pengacuhan|penghindaran|diam|a|Interogasi adalah proses pemeriksaan seseorang melalui pengajuan pertanyaan.
Interupsi artinya …|penyelaan, pemotongan|kelanjutan|penerusan|kesinambungan|tanpa henti|a|Interupsi adalah tindakan menyela atau memotong pembicaraan atau kegiatan orang lain.
Interval artinya …|jangka nada|keselarasan|harmoni|alunan penuh|keserasian|a|Dalam musik, interval adalah perbedaan ketinggian antara dua nada; jangka nada.
Intuisi artinya …|bisikan hati, gerakan hati|perhitungan logis|analisis|kalkulasi|rasionalitas|a|Intuisi adalah kemampuan memahami sesuatu tanpa melalui penalaran rasional; bisikan hati.
Intervensi artinya …|campur tangan|membiarkan|diam|tidak terlibat|mengabaikan|a|Intervensi adalah tindakan campur tangan dalam perselisihan antara dua pihak.
Iterasi artinya …|perulangan|sekali|tunggal|unik|satu kali|a|Iterasi adalah proses atau tindakan pengulangan.
Jurnal artinya …|surat kabar harian|novel|puisi|cerpen|dongeng|a|Jurnal bisa berarti buku catatan harian atau surat kabar yang terbit setiap hari.
Kaidah artinya …|patokan, aturan yang sudah pasti|kekacauan|kebiasaan|penyimpangan|kelonggaran|a|Kaidah adalah aturan atau patokan yang menjadi pedoman.
Kalkulasi artinya …|perhitungan, perincian biaya|tebakan|perkiraan kasar|dugaan|kira-kira|a|Kalkulasi adalah proses perhitungan angka atau biaya secara terperinci.
Kapitalis artinya …|kaum bermodal|kaum buruh|rakyat kecil|petani|pekerja|a|Kapitalis adalah sebutan untuk kaum pemilik modal besar dalam sistem ekonomi kapitalisme.
Kitab artinya …|buku|benda|batu|kayu|patung|a|Kitab secara umum berarti buku, terutama buku suci atau buku yang berisi ajaran penting.
Klan artinya …|suku, kelompok|individu|orang tunggal|pribadi|perorangan|a|Klan adalah kelompok kekerabatan yang besar berdasarkan satu garis keturunan; suku.
Klasifikasi artinya …|pengelompokan menurut kaidah|pencampuran|pengacakan|pengaburan|ketidakteraturan|a|Klasifikasi adalah proses pengelompokan sesuatu berdasarkan ciri-ciri atau kaidah tertentu.
Klimaks artinya …|puncak|dasar|lembah|rendah|titik bawah|a|Klimaks adalah titik puncak dari suatu kejadian, ketegangan, atau cerita.
Kognisi artinya …|pengenalan, penafsiran|ketidaktahuan|kebodohan|kealpaan|pengabaian|a|Kognisi adalah proses mental yang terlibat dalam memperoleh pengetahuan dan pemahaman melalui pikiran, pengalaman, dan indra.
Kolateral artinya …|paralel, sejalan, berdampingan, sejajar|berlawanan|menyilang|bertabrakan|berseberangan|a|Kolateral berarti sejajar atau berdampingan. Dalam keuangan, bisa berarti jaminan.
Koloni artinya …|jajahan|merdeka|bebas|berdaulat|independen|a|Koloni adalah daerah jajahan suatu negara.
Komoditas artinya …|barang dagangan utama|benda tak bernilai|barang sampah|limbah|buangan|a|Komoditas adalah barang dagangan utama yang diperjualbelikan, seperti hasil bumi atau tambang.
Kompleksitas artinya …|kerumitan|kesederhanaan|kemudahan|keteraturan|kelancaran|a|Kompleksitas adalah tingkat kerumitan atau kesukaran dari suatu sistem atau masalah.
Komplemen artinya …|pelengkap|kekurangan|ketidaklengkapan|kekosongan|kekurangan isi|a|Komplemen adalah sesuatu yang berfungsi sebagai pelengkap untuk menyempurnakan.
Konfrontasi artinya …|permusuhan, pertentangan|perdamaian|kerukunan|keselarasan|keharmonisan|a|Konfrontasi adalah keadaan permusuhan atau pertentangan secara terbuka.
Konkaf artinya …|cekung|cembung|menonjol|timbul|keluar|a|Konkaf adalah bentuk permukaan yang melengkung ke dalam seperti bagian dalam bola; cekung.
Konklusi artinya …|kesimpulan|kebingungan|praduga|keraguan|ketidakjelasan|a|Konklusi adalah simpulan atau pendapat akhir yang ditarik dari suatu uraian.
Konsesi artinya …|kerelaan|penolakan|keengganan|ketegaran|kekerasan hati|a|Konsesi adalah izin atau kerelaan yang diberikan oleh satu pihak kepada pihak lain, seringkali dalam negosiasi.
Konstitusi artinya …|undang-undang dasar suatu negara|kebiasaan|aturan adat|tradisi|norma tidak tertulis|a|Konstitusi adalah keseluruhan sistem aturan yang menetapkan dan mengatur pemerintahan suatu negara; undang-undang dasar.
Kontribusi artinya …|sumbangan|penarikan|pengambilan|perampasan|penyedotan|a|Kontribusi adalah sumbangan atau keikutsertaan dalam suatu kegiatan.
Konveks artinya …|cembung|cekung|melengkung ke dalam|masuk ke dalam|lekukan|a|Konveks adalah bentuk permukaan yang melengkung keluar seperti bagian luar bola; cembung.
Konvensional artinya …|tradisional|modern|baru|kontemporer|mutakhir|a|Konvensional berarti berdasarkan kebiasaan atau tradisi yang sudah umum dipakai.
Laten artinya …|tersembunyi, terpendam|tampak|terlihat|nyata|jelas|a|Laten berarti sesuatu yang ada tetapi tidak terlihat atau terpendam.
Majemuk artinya …|keanekaragaman|tunggal|satu|homogen|seragam|a|Majemuk berarti terdiri atas beberapa bagian yang merupakan kesatuan; menunjukkan keanekaragaman.
Makar artinya …|akal busuk, tipu muslihat|kejujuran|ketulusan|kebaikan|keterusterangan|a|Makar adalah usaha atau akal busuk untuk menjatuhkan pemerintahan yang sah; tipu muslihat.
Makna artinya …|arti|tanpa maksud|hampa|kosong|nihil|a|Makna adalah arti atau maksud yang terkandung dalam sebuah kata atau kalimat.
Manuskrip artinya …|naskah|hasil cetakan|buku jadi|terbitan modern|salinan digital|a|Manuskrip adalah naskah tulisan tangan yang menjadi dasar suatu kajian atau terbitan.
Masif artinya …|utuh, padat|rapuh|keropos|berlubang|retak|a|Masif berarti kuat, padat, utuh, dan murni (tidak berongga).
Materialistis artinya …|bersifat kebendaan|spiritual|rohaniah|batiniah|religius|a|Materialistis adalah pandangan hidup yang sangat mementingkan harta benda dan kekayaan.
Moderat artinya …|menghindari perilaku ekstrem|radikal|ekstremis|fanatik|keras|a|Moderat adalah sikap yang selalu menghindari perilaku atau pengungkapan yang ekstrem; berkecenderungan ke arah jalan tengah.
Monarki artinya …|kerajaan|republik|demokrasi|federasi|konfederasi|a|Monarki adalah bentuk pemerintahan yang dikepalai oleh seorang raja atau ratu.
Naluri artinya …|dorongan hati|perhitungan logis|analisis rasional|kajian mendalam|kalkulasi|a|Naluri adalah dorongan hati atau nafsu yang dibawa sejak lahir; insting.
Narasi artinya …|deskripsi|diam|kebisuan|keheningan|tanpa cerita|a|Narasi adalah pengisahan suatu cerita atau kejadian; deskripsi suatu peristiwa.
Navigasi artinya …|pelayaran, penerbangan|tersesat|kehilangan arah|kebingungan|salah jalan|a|Navigasi adalah ilmu tentang cara menjalankan kapal laut atau pesawat terbang; pelayaran.
Nisbi artinya …|relatif|mutlak|absolut|pasti|tidak berubah|a|Nisbi berarti tidak mutlak; relatif (tergantung pada pembandingnya).
Nomaden artinya …|berpindah-pindah|menetap|tinggal tetap|mapan|berdiam|a|Nomaden adalah cara hidup berpindah-pindah dari satu tempat ke tempat lain.
Nomenklatur artinya …|tata nama|tanpa nama|anonim|tidak bernama|sebutan kosong|a|Nomenklatur adalah sistem penamaan yang dipakai dalam bidang ilmu tertentu; tata nama.
Objektif artinya …|tidak dipengaruhi pendapat atau pandangan pribadi|subjektif|berpihak|memihak|berat sebelah|a|Objektif adalah penilaian yang didasarkan pada fakta, tanpa dipengaruhi pendapat pribadi.
Otoritas artinya …|kekuasaan, wewenang|kelemahan|ketidakberdayaan|tanpa kuasa|tidak berwenang|a|Otoritas adalah hak untuk memerintah atau kekuasaan yang sah untuk membuat keputusan.
Paradigma artinya …|kerangka berpikir|tanpa konsep|kekacauan|ketidakteraturan|kebingungan|a|Paradigma adalah model atau kerangka berpikir yang digunakan untuk menjelaskan suatu fenomena.
Paralel artinya …|sejajar|bersilangan|berpotongan|menyilang|berlawanan|a|Paralel berarti dua garis atau bidang yang berjarak sama di setiap titiknya; sejajar.
Pedagogi artinya …|ilmu pengajaran, ilmu pendidikan|kebodohan|ketidaktahuan|ketidakpedulian|buta huruf|a|Pedagogi adalah ilmu yang berkaitan dengan metode mengajar dan pendidikan.
`;

    /* ======= KONFIGURASI ======= */
    const POINT_PER_CORRECT = 5;
    const letters = ["a","b","c","d","e"];
    const THEME_KEY = 'quiz-theme';
    const LIMIT_KEY = 'quiz-limit';
    const ANSWERS_KEY = 'quiz-answers'; // NEW: Key for storing answers

    /* ======= Parsing data ======= */
    function parseData(raw){
      const lines = raw.trim().split(/\n+/);
      return lines.map((line, idx) => {
        const parts = line.split('|');
        if (parts.length < 7) throw new Error(`Format salah pada baris ${idx+1}`);
        const [text, A, B, C, D, E, key, explanation = ''] = parts;
        const answer = String(key).trim().toLowerCase();
        if (!/^[abcde]$/.test(answer)) throw new Error(`Kunci salah di baris ${idx+1}`);
        return { text, options:{a:A,b:B,c:C,d:D,e:E}, answer, explanation: explanation.trim() };
      });
    }
    const QUESTIONS = parseData(DATA);

    // ——— util ———
    const rand = (n) => Math.floor(Math.random()*n);
    const shuffleArray = (arr) => { for(let i=arr.length-1;i>0;i--){ const j=rand(i+1); [arr[i],arr[j]]=[arr[j],arr[i]] } return arr };
    function escapeHTML(str){return String(str).replace(/[&<>"']/g, s => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[s]))}
    const visibleIndices = () => state.order.slice(0, state.limit);

    // state
    let state = {
      shuffleOptions: true,
      order: [...Array(QUESTIONS.length).keys()],
      optionOrders: {},
      limit: 10
    };

    function buildOptionOrders() {
      state.optionOrders = {};
      state.order.forEach((qi) => {
        const opts = letters.slice();
        state.optionOrders[qi] = state.shuffleOptions ? shuffleArray(opts) : opts;
      });
    }

    function render() {
      const quiz = document.getElementById('quiz');
      quiz.innerHTML = '';
      document.getElementById('total').textContent = String(state.limit);
      
      const savedAnswers = JSON.parse(localStorage.getItem(ANSWERS_KEY) || '{}'); // NEW: Load answers

      visibleIndices().forEach((qi, visibleIdx) => {
        const q = QUESTIONS[qi];
        const optOrder = state.optionOrders[qi] || letters;

        const card = document.createElement('section');
        card.className = 'card';
        card.id = `q-${qi}`;

        const head = document.createElement('h3');
        head.className='qhead';
        head.innerHTML = `<span class="qno">No. ${visibleIdx+1}</span><span class="qtext">${escapeHTML(q.text)}</span>`;
        card.appendChild(head);

        const fs = document.createElement('fieldset');
        const choices = document.createElement('div');
        choices.className='choices';

        optOrder.forEach((letter) => {
          const id = `q${qi}-${letter}`;
          const label = document.createElement('label');
          label.className='choice';
          label.setAttribute('for', id);

          const input = document.createElement('input');
          input.type='radio';
          input.name=`q${qi}`;
          input.value=letter;
          input.id=id;
          input.required = true;

          // NEW: Check for saved answer and apply it
          if (savedAnswers[qi] === letter) {
            input.checked = true;
          }

          const key = letter.toUpperCase();
          const text = document.createElement('div');
          text.innerHTML = `<span class="badge">${key}</span> <span>${escapeHTML(q.options[letter])}</span>`;

          label.appendChild(input);
          label.appendChild(text);
          choices.appendChild(label);
        });

        fs.appendChild(choices);
        card.appendChild(fs);
        quiz.appendChild(card);
      });

      setScore(0, state.limit, 0);
      document.getElementById('summary').hidden = true;
    }

    function setScore(correct, total, points) {
      const pct = total ? Math.round((correct/total)*100) : 0;
      document.getElementById('score').textContent = String(correct);
      document.getElementById('percent').textContent = String(pct);
      document.getElementById('points').textContent = String(points);
      const bar = document.getElementById('barfill');
      bar.style.width = pct + '%';
      bar.setAttribute('aria-valuenow', String(pct));
    }

    function grade() {
      const form = document.getElementById('quiz');
      const firstInvalid = form.querySelector(':invalid');
      if (firstInvalid) {
        firstInvalid.closest('.card').scrollIntoView({behavior:'smooth', block:'center'});
        firstInvalid.closest('.card').style.borderColor = 'var(--bad)';
        setTimeout(() => { firstInvalid.closest('.card').style.borderColor = 'var(--line)'; }, 2000);
        return;
      }

      let correct = 0;
      const fd = new FormData(form);

      visibleIndices().forEach((qi) => {
        const q = QUESTIONS[qi];
        const selected = fd.get(`q${qi}`);
        const card = document.getElementById(`q-${qi}`);
        
        card.classList.remove('correct','wrong');
        card.querySelectorAll('label').forEach(l => l.classList.remove('correct-answer-label', 'wrong-answer-label'));

        const isCorrect = selected === q.answer;
        if (isCorrect) { 
            correct++; 
            card.classList.add('correct');
        } else { 
            card.classList.add('wrong'); 
        }

        const correctLabel = card.querySelector(`label[for="q${qi}-${q.answer}"]`);
        if(correctLabel) correctLabel.classList.add('correct-answer-label');

        if (!isCorrect && selected) {
            const selectedLabel = card.querySelector(`label[for="q${qi}-${selected}"]`);
            if(selectedLabel) selectedLabel.classList.add('wrong-answer-label');
        }

        card.querySelectorAll('input[type="radio"]').forEach(input => input.disabled = true);
      });

      const points = correct * POINT_PER_CORRECT;
      setScore(correct, state.limit, points);
      window.scrollTo({top:0,behavior:'smooth'});

      buildSummary(fd, correct, points);
    }

    function buildSummary(fd, correct, points) {
      const tbody = document.querySelector('#sum-table tbody');
      tbody.innerHTML = '';
      let answered = 0;

      visibleIndices().forEach((qi, visibleIdx) => {
        const q = QUESTIONS[qi];
        const sel = fd.get(`q${qi}`);
        const isAnswered = !!sel;
        if (isAnswered) answered++;
        const isCorrect = sel === q.answer;
        const yourText = isAnswered ? `${sel.toUpperCase()}. ${q.options[sel]}` : 'Belum dijawab';
        const keyText  = `${q.answer.toUpperCase()}. ${q.options[q.answer]}`;
        const pts = isCorrect ? POINT_PER_CORRECT : 0;

        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${visibleIdx+1}</td>
          <td>${escapeHTML(q.text)}</td>
          <td>${escapeHTML(yourText)}</td>
          <td>${escapeHTML(keyText)}</td>
          <td class="${isCorrect?'status-ok':'status-bad'}">${isCorrect?'Benar':'Salah'}</td>
          <td class="${isCorrect?'status-ok':'status-bad'}"><strong>${pts}</strong></td>
        `;
        tbody.appendChild(tr);

        if (q.explanation) {
            const trExp = document.createElement('tr');
            trExp.className = 'explanation-row';
            trExp.innerHTML = `<td colspan="6" class="explanation-cell"><strong>Pembahasan:</strong> ${escapeHTML(q.explanation)}</td>`;
            tbody.appendChild(trExp);
        }
      });

      document.getElementById('sum-answered').textContent = answered;
      document.getElementById('sum-correct').textContent = correct;
      document.getElementById('sum-points').textContent = points;
      document.getElementById('summary').hidden = false;
      document.getElementById('summary').scrollIntoView({behavior:'smooth', block:'start'});
    }

    function resetAll() {
      localStorage.removeItem(ANSWERS_KEY); // NEW: Clear saved answers
      render();
    }

    function shuffleQuestions() {
      localStorage.removeItem(ANSWERS_KEY); // NEW: Clear saved answers
      state.order = shuffleArray(state.order);
      buildOptionOrders();
      render();
    }

    function toggleShuffleOptions() {
      state.shuffleOptions = !state.shuffleOptions;
      document.getElementById('optState').textContent = state.shuffleOptions ? 'ON' : 'OFF';
      buildOptionOrders();
      render();
    }

    // NEW: Save selected answer to localStorage
    function handleAnswerChange(e) {
        if (e.target.tagName !== 'INPUT' || e.target.type !== 'radio') return;
        const answers = JSON.parse(localStorage.getItem(ANSWERS_KEY) || '{}');
        const questionIndex = e.target.name.replace('q', '');
        answers[questionIndex] = e.target.value;
        localStorage.setItem(ANSWERS_KEY, JSON.stringify(answers));
    }

    // ====== THEME TOGGLE ======
    function setTheme(theme){
      document.documentElement.setAttribute('data-theme', theme);
      localStorage.setItem(THEME_KEY, theme);
    }
    function initTheme(){
      const saved = localStorage.getItem(THEME_KEY);
      if (saved === 'light' || saved === 'dark') { setTheme(saved); return; }
      const prefersLight = window.matchMedia && window.matchMedia('(prefers-color-scheme: light)').matches;
      setTheme(prefersLight ? 'light' : 'dark');
    }

    // ====== LIMIT (Jumlah Soal) ======
    function initLimit(){
      const saved = parseInt(localStorage.getItem(LIMIT_KEY) || '10', 10);
      state.limit = [10,20,30,40,50,60,70,80,90,100,110,120,130,140,151].includes(saved) ? saved : 10;
      const sel = document.getElementById('limitSelect');
      sel.value = String(state.limit);
      sel.addEventListener('change', (e)=>{
        localStorage.removeItem(ANSWERS_KEY); // NEW: Clear saved answers on limit change
        state.limit = parseInt(e.target.value,10);
        localStorage.setItem(LIMIT_KEY, String(state.limit));
        render();
      });
    }

    // ====== BOOTSTRAP ======
    (function init(){
      initTheme();
      initLimit();
      state.order = [...Array(QUESTIONS.length).keys()];
      buildOptionOrders();
      render();
    })();

    // ====== EVENT LISTENERS ======
    document.getElementById('quiz').addEventListener('change', handleAnswerChange);
    document.getElementById('grade').addEventListener('click', (e) => { e.preventDefault(); grade(); });
    document.getElementById('reset').addEventListener('click', (e)=>{ e.preventDefault(); resetAll(); });
    document.getElementById('shuffleQuestions').addEventListener('click', (e)=>{ e.preventDefault(); shuffleQuestions(); });
    document.getElementById('shuffleOptions').addEventListener('click', (e)=>{ e.preventDefault(); toggleShuffleOptions(); });
    document.getElementById('modeToggle').addEventListener('click', (e)=>{ 
      e.preventDefault(); 
      const cur = document.documentElement.getAttribute('data-theme') || 'dark';
      setTheme(cur === 'dark' ? 'light' : 'dark');
    });
  </script>
</body>
</html>


