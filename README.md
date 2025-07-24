<!DOCTYPE html>
<html lang="id">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daftar Belanja Hari Ini</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

    body {
      font-family: 'Inter', sans-serif;
    }
  </style>
</head>

<body class="bg-gradient-to-br from-orange-50 to-red-50 min-h-screen">
  <div class="container mx-auto px-4 py-8 max-w-4xl">
    <!-- Header -->
    <div class="text-center mb-8">
      <h1 class="text-4xl font-bold text-gray-800 mb-2">🍗 Daftar Belanja Hari Ini</h1>
      <h1 class="text-0.5xl font-text-gray-800 mb-1">by .O2</h1>
      <p class="text-gray-600">Sesuaikan porsi dan dapatkan daftar belanja yang tepat!</p>
    </div>

    <!-- Custom Item Input -->
    <div class="bg-white rounded-2xl shadow-lg p-6 mb-6">
      <h2 class="text-2xl font-bold text-gray-800 mb-4 flex items-center">
        ✏️ Tambah Item Sendiri
      </h2>
      <p class="text-gray-600 mb-4">Masukkan item belanja tambahan yang Anda butuhkan. Jumlah akan otomatis menyesuaikan
        dengan porsi yang dipilih.</p>
      <div class="grid md:grid-cols-4 gap-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Nama Item</label>
          <input type="text" id="customItemName" placeholder="Contoh: Bumbu rendang"
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-orange-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Jumlah</label>
          <input type="number" id="customItemAmount" placeholder="2" min="0.1" step="0.1"
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-orange-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Satuan</label>
          <select id="customItemUnit"
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-orange-500">
            <option value="Buah">Buah</option>
            <option value="Kg">Kg</option>
            <option value="Gram">Gram (Gr)</option>
            <option value="Liter">Liter (L)</option>
            <option value="ml">ml</option>
            <option value="Sdm">Sdm</option>
            <option value="Sdt">Sdt</option>
            <option value="Bungkus">Bungkus</option>
            <option value="Sachet">Sachet</option>
            <option value="Botol">Botol</option>
            <option value="Renteng">Renteng</option>
            <option value="Ikat">Ikat</option>
          </select>
        </div>
        <div class="flex items-end">
          <button onclick="addCustomItem()"
            class="w-full bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-semibold transition-colors">
            Tambah
          </button>
        </div>
      </div>
    </div>

    <!-- Custom Items List -->
    <div id="customItemsSection" class="bg-white rounded-2xl shadow-lg p-6 mb-6 hidden">
      <h2 class="text-2xl font-bold text-gray-800 mb-4 flex items-center">
        📝 Item Tambahan Anda
      </h2>
      <div class="space-y-3" id="customItemsList">
        <!-- Custom items will be added here -->
      </div>
    </div>





    <!-- Action Buttons -->
    <div class="mt-8 flex justify-center space-x-4">
      <button onclick="downloadPDF()"
        class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold transition-colors flex items-center space-x-2">
        <span>📄</span>
        <span>Unduh PDF</span>
      </button>
      <button onclick="resetList()"
        class="bg-gray-500 hover:bg-gray-600 text-white px-6 py-3 rounded-lg font-semibold transition-colors flex items-center space-x-2">
        <span>🔄</span>
        <span>Reset</span>
      </button>
    </div>
  </div>

  <script>
    let customItems = [];

    function formatAmount(amount, unit) {
      return `${amount} ${unit}`;
    }

    function downloadPDF() {
      if (customItems.length === 0) {
        alert('Mohon tambahkan item terlebih dahulu!');
        return;
      }

      const {jsPDF} = window.jspdf;
      const doc = new jsPDF();

      // Title
      doc.setFontSize(20);
      doc.text('Daftar Belanja Hari Ini', 20, 30);

      // Date
      doc.setFontSize(12);
      const today = new Date().toLocaleDateString('id-ID');
      doc.text(`Tanggal: ${today}`, 20, 45);

      // Items
      doc.setFontSize(14);
      doc.text('Item Belanja:', 20, 65);

      let yPosition = 80;
      customItems.forEach((item, index) => {
        const itemText = `${index + 1}. ${item.name} - ${item.amount} ${item.unit}`;
        doc.setFontSize(12);
        doc.text(itemText, 25, yPosition);
        yPosition += 15;

        // Add new page if needed
        if (yPosition > 270) {
          doc.addPage();
          yPosition = 30;
        }
      });

      // Save the PDF
      doc.save('Daftar-Belanja-Hari-Ini.pdf');
    }

    function addCustomItem() {
      const name = document.getElementById('customItemName').value.trim();
      const amount = parseFloat(document.getElementById('customItemAmount').value);
      const unit = document.getElementById('customItemUnit').value;

      if (!name || !amount || amount <= 0) {
        alert('Mohon isi nama item dan jumlah yang valid!');
        return;
      }

      const customItem = {
        id: Date.now(),
        name: name,
        amount: amount,
        unit: unit,
        emoji: '📦'
      };

      customItems.push(customItem);
      updateCustomItemsList();

      // Clear form
      document.getElementById('customItemName').value = '';
      document.getElementById('customItemAmount').value = '';
      document.getElementById('customItemUnit').selectedIndex = 0;
    }

    function removeCustomItem(id) {
      customItems = customItems.filter(item => item.id !== id);
      updateCustomItemsList();
    }

    function updateCustomItemsList() {
      const customSection = document.getElementById('customItemsSection');
      const customList = document.getElementById('customItemsList');

      if (customItems.length === 0) {
        customSection.classList.add('hidden');
        return;
      }

      customSection.classList.remove('hidden');
      customList.innerHTML = customItems.map(item => {
        const formattedAmount = formatAmount(item.amount, item.unit);

        return `
                    <div class="flex items-center space-x-3 p-3 bg-orange-50 rounded-lg hover:bg-orange-100 transition-colors">
                        <input type="checkbox" class="w-5 h-5 text-orange-500 rounded focus:ring-orange-500">
                        <span class="text-2xl">${item.emoji}</span>
                        <div class="flex-1">
                            <span class="font-medium text-gray-800">${item.name}</span>
                            <span class="text-orange-600 font-semibold ml-2">${formattedAmount}</span>
                        </div>
                        <button onclick="removeCustomItem(${item.id})" class="text-red-500 hover:text-red-700 font-bold text-lg">×</button>
                    </div>
                `;
      }).join('');
    }

    function resetList() {
      // Uncheck all checkboxes
      const checkboxes = document.querySelectorAll('input[type="checkbox"]');
      checkboxes.forEach(checkbox => checkbox.checked = false);

      // Clear custom items
      customItems = [];
      updateCustomItemsList();
    }
  </script>
  <script>(function () {function c() {var b = a.contentDocument || a.contentWindow.document; if (b) {var d = b.createElement('script'); d.innerHTML = "window.__CF$cv$params={r:'964201bf33c95d17',t:'MTc1MzM0NTMxNi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);"; b.getElementsByTagName('head')[0].appendChild(d)} } if (document.body) {var a = document.createElement('iframe'); a.height = 1; a.width = 1; a.style.position = 'absolute'; a.style.top = 0; a.style.left = 0; a.style.border = 'none'; a.style.visibility = 'hidden'; document.body.appendChild(a); if ('loading' !== document.readyState) c(); else if (window.addEventListener) document.addEventListener('DOMContentLoaded', c); else {var e = document.onreadystatechange || function () { }; document.onreadystatechange = function (b) {e(b); 'loading' !== document.readyState && (document.onreadystatechange = e, c())}} } })();</script>
</body>

</html>
