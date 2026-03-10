<!doctype html>
<html lang="id" class="h-full">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PPDB SDN PENGKOL - REKAP LENGKAP</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: { primary: '#0F52BA', secondary: '#F8FAFC', accent: '#1E40AF' },
          fontFamily: { jakarta: ['Plus Jakarta Sans', 'sans-serif'] }
        }
      }
    }
  </script>
  <style>
    body { background-color: #F8FAFC; color: #1E293B; font-family: 'Plus Jakarta Sans', sans-serif; }
    .glass-card { background: white; border: 1px solid rgba(15, 82, 186, 0.1); box-shadow: 0 4px 20px -2px rgba(0, 0, 0, 0.05); }
    #login-overlay { 
      /* Menggunakan foto IMG_0557.JPG dari Google Drive Anda */
      background-image: linear-gradient(rgba(10, 70, 160, 0.6), rgba(10, 82, 186, 0.85)), 
      url('https://lh3.googleusercontent.com/d/1c9n9sRg01Lxg7YlC_ww1lFoumBZDLrnW');
      
      background-size: cover; 
      background-position: center;
      background-repeat: no-repeat;
      background-attachment: fixed;
    }
    .sidebar-active { background: linear-gradient(135deg, #0F52BA 0%, #1E40AF 100%); color: white !important; box-shadow: 0 10px 15px -3px rgba(15, 82, 186, 0.3); }
    .input-field { width: 100%; margin-top: 4px; padding: 10px 14px; border-radius: 10px; border: 1px solid #E2E8F0; background: #F8FAFC; font-size: 13px; transition: all 0.2s; }
    .input-field:focus { border-color: #0F52BA; outline: none; background: white; }
    label { font-size: 10px; font-weight: 800; color: #64748B; text-transform: uppercase; letter-spacing: 0.5px; }
    section-title { display: block; font-size: 13px; font-weight: 800; color: #0F52BA; border-bottom: 2px solid #F1F5F9; padding-bottom: 8px; margin-bottom: 16px; margin-top: 24px; text-transform: uppercase; }
    @media print { .no-print { display: none !important; } }
  </style>
</head>

<body class="h-full">
  <div id="app" class="min-h-screen flex flex-col">
    <div id="login-overlay" class="fixed inset-0 z-[100] flex items-center justify-center p-6">
      <div class="bg-white/95 backdrop-blur-sm p-8 rounded-3xl shadow-2xl w-full max-w-md">
        <h1 class="text-2xl font-black text-primary text-center uppercase">PPDB SDN PENGKOL</h1>
        <p class="text-slate-500 mb-8 text-center text-[10px] font-bold uppercase italic">Sistem Penerimaan Siswa Baru</p>
        <div class="space-y-4">
          <label>Username</label><input type="text" id="username" class="input-field mb-2" placeholder="admin">
          <label>Password</label><input type="password" id="password" class="input-field mb-4" placeholder="••••••••">
          <button onclick="login()" class="w-full py-4 bg-primary text-white rounded-xl font-bold hover:bg-accent transition-all uppercase text-xs tracking-widest shadow-lg shadow-blue-100">Login</button>
        </div>
      </div>
    </div>

    <div id="main-content" class="hidden flex-1 flex flex-col">
      <nav class="bg-white border-b border-slate-100 sticky top-0 z-40 px-8 h-20 flex justify-between items-center no-print">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 bg-primary rounded-xl flex items-center justify-center text-white font-black text-xl shadow-lg shadow-blue-100">P</div>
          <span class="text-lg font-black text-primary uppercase">SDN PENGKOL</span>
        </div>
        <button onclick="location.reload()" class="text-[10px] font-black text-red-500 hover:bg-red-50 px-4 py-2 rounded-full border border-red-100 uppercase">Keluar</button>
      </nav>

      <div class="flex-1 max-w-7xl mx-auto w-full p-4 lg:p-8 flex flex-col lg:flex-row gap-8">
        <aside class="w-full lg:w-64 flex flex-row lg:flex-col gap-2 no-print">
          <button onclick="showPage('dashboard')" id="btn-dashboard" class="sidebar-active px-6 py-4 rounded-2xl font-bold text-left">Dashboard</button>
          <button onclick="showPage('input')" id="btn-input" class="text-slate-500 px-6 py-4 rounded-2xl font-bold text-left hover:bg-white">Input Data</button>
          <button onclick="showPage('rekap')" id="btn-rekap" class="text-slate-500 px-6 py-4 rounded-2xl font-bold text-left hover:bg-white">Rekapitulasi</button>
        </aside>

        <div class="flex-1">
          <div id="page-dashboard" class="space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <div class="glass-card p-5 rounded-3xl border-l-4 border-l-primary">
                <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Total Pendaftar</p>
                <h3 id="stat-total" class="text-3xl font-black text-primary">0</h3>
              </div>
              <div class="glass-card p-5 rounded-3xl border-l-4 border-l-blue-400">
                <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Laki-Laki</p>
                <h3 id="stat-l" class="text-3xl font-black text-blue-500">0</h3>
              </div>
              <div class="glass-card p-5 rounded-3xl border-l-4 border-l-rose-400">
                <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Perempuan</p>
                <h3 id="stat-p" class="text-3xl font-black text-rose-500">0</h3>
              </div>
              <div class="glass-card p-5 rounded-3xl border-l-4 border-l-emerald-400 relative">
                <p class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Sisa Kuota</p>
                <h3 id="stat-sisa" class="text-3xl font-black text-emerald-500">84</h3>
                <button onclick="editKuota()" class="absolute top-2 right-2 text-primary hover:bg-blue-50 p-2 rounded-lg"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z"/></svg></button>
              </div>
            </div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <div class="glass-card p-6 rounded-3xl">
                    <h4 class="text-[10px] font-black text-slate-800 uppercase mb-4">Grafik Pendaftaran Mingguan</h4>
                    <div id="chart-container" class="h-40 flex items-end justify-between gap-2 px-2"></div>
                </div>
                <div class="glass-card p-6 rounded-3xl">
                    <h4 class="text-[10px] font-black text-slate-800 uppercase mb-4">Aktivitas Terkini</h4>
                    <div id="recent-list" class="space-y-3"></div>
                </div>
            </div>
          </div>

          <div id="page-input" class="hidden">
            <div class="glass-card p-8 rounded-3xl">
              <h2 class="text-xl font-black text-slate-800 mb-6 uppercase">Pendaftaran Siswa Baru TA 2026/2027</h2>
              <form id="ppdb-form" onsubmit="handleFormSubmit(event)">
                <section-title>I. Biodata Calon Siswa</section-title>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                  <div class="lg:col-span-2"><label>1. Nama Lengkap</label><input type="text" name="nama" required class="input-field" placeholder="HURUF KAPITAL"></div>
                  <div><label>2. NIK</label><input type="text" name="nik" required class="input-field" maxlength="16"></div>
                  <div><label>3. Jenis Kelamin</label><select name="jk" class="input-field"><option>Laki-laki</option><option>Perempuan</option></select></div>
                  <div><label>4. Tempat Lahir</label><input type="text" name="tempat_lahir" required class="input-field"></div>
                  <div><label>5. Tanggal Lahir</label><input type="date" name="tgl_lahir" onchange="previewUsia(this.value)" required class="input-field"><p id="usia-preview" class="text-[10px] text-primary font-bold mt-1 uppercase"></p></div>
                  <div><label>6. Agama</label><select name="agama" class="input-field"><option>Islam</option><option>Kristen</option><option>Katolik</option><option>Hindu</option><option>Budha</option></select></div>
                  <div class="lg:col-span-2"><label>7. Alamat Lengkap (Dusun/RT/RW/Desa)</label><input type="text" name="alamat" required class="input-field"></div>
                  <div><label>8. Asal Sekolah (TK/PAUD)</label><input type="text" name="asal_sekolah" class="input-field"></div>
                </div>

                <section-title>II. Data Orang Tua / Wali</section-title>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 border-l-2 border-primary/20 pl-4">
                  <div class="space-y-4">
                    <div><label>Nama Ayah Kandung</label><input type="text" name="nama_ayah" required class="input-field"></div>
                    <div><label>Pendidikan Terakhir Ayah</label><select name="pendidikan_ayah" class="input-field"><option>SD/SMP/SMA</option><option>D3/S1</option><option>S2/S3</option><option>Tidak Sekolah</option></select></div>
                    <div><label>Pekerjaan Ayah</label><input type="text" name="pekerjaan_ayah" class="input-field"></div>
                  </div>
                  <div class="space-y-4">
                    <div><label>Nama Ibu Kandung</label><input type="text" name="nama_ibu" required class="input-field"></div>
                    <div><label>Pendidikan Terakhir Ibu</label><select name="pendidikan_ibu" class="input-field"><option>SD/SMP/SMA</option><option>D3/S1</option><option>S2/S3</option><option>Tidak Sekolah</option></select></div>
                    <div><label>Pekerjaan Ibu</label><input type="text" name="pekerjaan_ibu" class="input-field"></div>
                  </div>
                  <div class="md:col-span-2"><label>Nomor HP / WhatsApp Aktif</label><input type="tel" name="no_hp" required class="input-field" placeholder="08xxxxxxxxxx"></div>
                </div>

                <section-title>III. Administrasi Jalur & Petugas</section-title>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 bg-primary/5 p-5 rounded-2xl border border-primary/10">
                  <div><label class="text-primary">Jalur</label><select name="jalur" class="input-field border-primary/20"><option>Zonasi</option><option>Afirmasi</option><option>Prestasi</option><option>Perpindahan</option></select></div>
                  <div><label class="text-primary">Nama Petugas</label><input type="text" name="petugas_nama" required class="input-field border-primary/20"></div>
                  <div><label class="text-primary">NIP</label><input type="text" name="petugas_nip" class="input-field border-primary/20"></div>
                </div>

                <div class="mt-10 flex gap-4">
                  <button type="submit" class="flex-1 py-4 bg-primary text-white rounded-2xl font-black shadow-xl hover:bg-accent transition-all uppercase text-xs tracking-widest">Simpan & Cetak Bukti</button>
                  <button type="reset" class="px-8 py-4 bg-slate-100 text-slate-400 rounded-2xl font-bold hover:bg-slate-200 uppercase text-xs">Reset</button>
                </div>
              </form>
            </div>
          </div>

          <div id="page-rekap" class="hidden space-y-4">
            <div class="flex justify-between items-center no-print">
               <h3 class="text-sm font-black text-slate-800 uppercase">Rekapitulasi Calon Siswa</h3>
               <div class="flex gap-2">
                 <button onclick="exportExcel()" class="px-4 py-2 bg-emerald-600 text-white rounded-xl text-[9px] font-black shadow-lg">EXCEL</button>
                 <button onclick="exportPDF()" class="px-4 py-2 bg-rose-600 text-white rounded-xl text-[9px] font-black shadow-lg">PDF</button>
               </div>
            </div>
            <div class="glass-card rounded-3xl overflow-hidden overflow-x-auto">
              <table id="table-rekap" class="w-full text-left">
                <thead class="bg-slate-50 border-b border-slate-100 whitespace-nowrap">
                  <tr class="text-[9px] font-black text-slate-400 uppercase tracking-widest">
                    <th class="px-4 py-4">Reg</th>
                    <th class="px-4 py-4">Nama Siswa</th>
                    <th class="px-4 py-4">NIK</th>
                    <th class="px-4 py-4">Jenis Kelamin</th>
                    <th class="px-4 py-4">Alamat</th>
                    <th class="px-4 py-4">Asal TK</th>
                    <th class="px-4 py-4">Ibu Kandung</th>
                    <th class="px-4 py-4">Ayah Kandung</th>
                    <th class="px-4 py-4">Aksi</th>
                  </tr>
                </thead>
                <tbody id="rekap-table-body" class="divide-y divide-slate-50 text-[10px]"></tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    let registrationData = JSON.parse(localStorage.getItem('ppdb_data')) || [];
    let PAGU_KUOTA = localStorage.getItem('ppdb_pagu') || 84;
    const NPSN = "20402647";

    function login() {
      if (document.getElementById('username').value !== "") {
        document.getElementById('login-overlay').classList.add('hidden');
        document.getElementById('main-content').classList.remove('hidden');
        updateDashboard();
      }
    }

    function showPage(id) {
      ['dashboard', 'input', 'rekap'].forEach(p => {
        document.getElementById(`page-${p}`).classList.add('hidden');
        document.getElementById(`btn-${p}`).classList.remove('sidebar-active');
      });
      document.getElementById(`page-${id}`).classList.remove('hidden');
      document.getElementById(`btn-${id}`).classList.add('sidebar-active');
      if (id === 'rekap') renderTable();
      if (id === 'dashboard') updateDashboard();
    }

    function editKuota() {
      const v = prompt("Ubah Pagu Kuota:", PAGU_KUOTA);
      if (v) { PAGU_KUOTA = v; localStorage.setItem('ppdb_pagu', v); updateDashboard(); }
    }

    function calculateUsia(dateStr) {
      const target = new Date('2024-07-01');
      const birth = new Date(dateStr);
      let y = target.getFullYear() - birth.getFullYear();
      let m = target.getMonth() - birth.getMonth();
      if (m < 0) { y--; m += 12; }
      return { text: `${y} Thn, ${m} Bln` };
    }

    function previewUsia(val) {
      if (!val) return;
      document.getElementById('usia-preview').innerText = "Usia 1 Juli: " + calculateUsia(val).text;
    }

    function handleFormSubmit(e) {
      e.preventDefault();
      const f = new FormData(e.target);
      const reg = `${NPSN}-${String(registrationData.length + 1).padStart(3, '0')}`;

      const entry = {
        reg: reg,
        nama: f.get('nama').toUpperCase(),
        nik: f.get('nik'),
        jk: f.get('jk'),
        ttl: `${f.get('tempat_lahir')}, ${f.get('tgl_lahir')}`,
        agama: f.get('agama'),
        alamat: f.get('alamat'),
        asal_sekolah: f.get('asal_sekolah') || '-',
        nama_ayah: f.get('nama_ayah').toUpperCase(),
        pend_ayah: f.get('pendidikan_ayah'),
        pekerjaan_ayah: f.get('pekerjaan_ayah'),
        nama_ibu: f.get('nama_ibu').toUpperCase(),
        pend_ibu: f.get('pendidikan_ibu'),
        pekerjaan_ibu: f.get('pekerjaan_ibu'),
        no_hp: f.get('no_hp'),
        jalur: f.get('jalur'),
        petugas: f.get('petugas_nama'),
        nip: f.get('petugas_nip'),
        usia: calculateUsia(f.get('tgl_lahir')).text,
        waktu: new Date().toLocaleTimeString('id-ID', {hour:'2-digit', minute:'2-digit'})
      };

      registrationData.push(entry);
      localStorage.setItem('ppdb_data', JSON.stringify(registrationData));
      alert("BERHASIL DISIMPAN!");
      e.target.reset();
      updateDashboard();
      showPage('dashboard');
    }

    function updateDashboard() {
      const total = registrationData.length;
      const lakilaki = registrationData.filter(x => x.jk === 'Laki-laki').length;
      const perempuan = registrationData.filter(x => x.jk === 'Perempuan').length;

      document.getElementById('stat-total').innerText = total;
      document.getElementById('stat-l').innerText = lakilaki;
      document.getElementById('stat-p').innerText = perempuan;
      document.getElementById('stat-sisa').innerText = PAGU_KUOTA - total;
      
      const chart = document.getElementById('chart-container');
      chart.innerHTML = ['Sen', 'Sel', 'Rab', 'Kam', 'Jum', 'Sab', 'Min'].map(day => {
        const h = registrationData.length > 0 ? Math.floor(Math.random() * 80) + 10 : 5;
        return `<div class="flex-1 flex flex-col items-center gap-2 group">
                  <div class="w-full bg-primary/10 rounded-t-lg group-hover:bg-primary transition-all" style="height:${h}%"></div>
                  <span class="text-[8px] font-black text-slate-300">${day}</span>
                </div>`;
      }).join('');

      const list = document.getElementById('recent-list');
      const recent = [...registrationData].reverse().slice(0, 3);
      list.innerHTML = recent.length ? recent.map(r => `
        <div class="flex justify-between items-center p-3 bg-slate-50 rounded-2xl border border-slate-100">
          <div><p class="text-[10px] font-black uppercase">${r.nama}</p><p class="text-[8px] text-slate-400 font-bold">${r.jalur} • ${r.waktu}</p></div>
          <span class="text-[8px] font-bold ${r.jk === 'Laki-laki' ? 'text-blue-500' : 'text-rose-500'} uppercase">${r.jk[0]}</span>
        </div>
      `).join('') : '<p class="text-center py-4 text-[10px] text-slate-300 italic">Belum ada data</p>';
    }

    function renderTable() {
      const tbody = document.getElementById('rekap-table-body');
      tbody.innerHTML = registrationData.map((item, idx) => `
        <tr class="hover:bg-slate-50 transition-all">
          <td class="px-4 py-3 font-black text-primary">${item.reg}</td>
          <td class="px-4 py-3 font-bold uppercase">${item.nama}</td>
          <td class="px-4 py-3 font-mono text-slate-500">${item.nik}</td>
          <td class="px-4 py-3">${item.jk}</td>
          <td class="px-4 py-3 truncate max-w-[150px]" title="${item.alamat}">${item.alamat}</td>
          <td class="px-4 py-3 uppercase">${item.asal_sekolah}</td>
          <td class="px-4 py-3 font-bold uppercase">${item.nama_ibu}</td>
          <td class="px-4 py-3 font-bold uppercase">${item.nama_ayah}</td>
          <td class="px-4 py-3 flex gap-2">
            <button onclick="printA4('${item.reg}')" class="text-primary font-black underline uppercase text-[8px]">Print</button>
            <button onclick="hapusData(${idx})" class="text-red-500 font-black uppercase text-[8px]">X</button>
          </td>
        </tr>
      `).join('');
    }

    function hapusData(i) {
      if(confirm("Hapus data permanen?")) {
        registrationData.splice(i, 1);
        localStorage.setItem('ppdb_data', JSON.stringify(registrationData));
        renderTable(); updateDashboard();
      }
    }

    function exportExcel() {
      // Mapping kolom agar urutan sesuai permintaan di Excel
      const excelData = registrationData.map(item => ({
        "No. Registrasi": item.reg,
        "Nama Siswa": item.nama,
        "NIK": item.nik,
        "Jenis Kelamin": item.jk,
        "Alamat": item.alamat,
        "Asal TK": item.asal_sekolah,
        "Nama Ibu Kandung": item.nama_ibu,
        "Nama Ayah Kandung": item.nama_ayah
      }));
      const ws = XLSX.utils.json_to_sheet(excelData);
      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "Rekap_PPDB");
      XLSX.writeFile(wb, "REKAP_PPDB_SDN_PENGKOL_2026.xlsx");
    }

    function exportPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF('l', 'mm', 'a4');
      doc.setFontSize(12);
      doc.text("REKAPITULASI PENDAFTARAN PPDB SDN PENGKOL 2026", 14, 15);
      doc.autoTable({
        head: [['Reg', 'Nama Siswa', 'NIK', 'JK', 'Alamat', 'Asal TK', 'Ibu', 'Ayah']],
        body: registrationData.map(d => [d.reg, d.nama, d.nik, d.jk, d.alamat, d.asal_sekolah, d.nama_ibu, d.nama_ayah]),
        startY: 20,
        styles: { fontSize: 7, cellPadding: 2 }
      });
      doc.save("REKAP_PPDB_LENGKAP.pdf");
    }

    function printA4(reg) {
      const item = registrationData.find(x => x.reg === reg);
      const win = window.open('', '_blank');
      win.document.write(`
        <html><head><style>@page{size:A4;margin:0}body{font-family:sans-serif;padding:25mm;color:#1e293b}.hdr{background:#0F52BA;color:white;padding:30px;border-radius:15px;text-align:center;margin-bottom:30px}.row{display:flex;margin-bottom:12px;border-bottom:1px solid #f1f5f9;padding-bottom:5px;font-size:12px}.lbl{width:200px;font-weight:bold;color:#64748b}.val{flex:1;text-transform:uppercase;font-weight:bold}</style></head>
        <body onload="window.print()">
          <div class="hdr"><h2>BUKTI PENDAFTARAN PPDB SDN PENGKOL</h2><p>Tahun Pelajaran 2026/2027</p></div>
          <div class="row"><div class="lbl">NO REGISTRASI</div><div class="val">: ${item.reg}</div></div>
          <div class="row"><div class="lbl">NAMA LENGKAP SISWA</div><div class="val">: ${item.nama}</div></div>
          <div class="row"><div class="lbl">NIK SISWA</div><div class="val">: ${item.nik}</div></div>
          <div class="row"><div class="lbl">TEMPAT, TGL LAHIR</div><div class="val">: ${item.ttl}</div></div>
          <div class="row"><div class="lbl">JENIS KELAMIN</div><div class="val">: ${item.jk}</div></div>
          <div class="row"><div class="lbl">NAMA AYAH KANDUNG</div><div class="val">: ${item.nama_ayah}</div></div>
          <div class="row"><div class="lbl">NAMA IBU KANDUNG</div><div class="val">: ${item.nama_ibu}</div></div>
          <div class="row"><div class="lbl">NOMOR HP WALI</div><div class="val">: ${item.no_hp}</div></div>
          <div class="row"><div class="lbl">ALAMAT</div><div class="val">: ${item.alamat}</div></div>
          <div style="margin-top:50px;float:right;text-align:center">
            <p>Petugas Pendaftaran,</p><br><br><br>
            <p><strong>${item.petugas}</strong><br>NIP. ${item.nip}</p>
          </div>
        </body></html>
      `);
      win.document.close();
    }
  </script>
</body>
</html>
