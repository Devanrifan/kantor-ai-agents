# Kantor AI

Kantor isometrik 3D berisi 23 karyawan AI untuk digital agency. Klik karakter, lalu ngobrol langsung dengan orangnya.

**Isi kantor** (satu ruangan per divisi):

| Divisi | Tim |
|---|---|
| Proposal Writer | Rina (leader), Bagas, Siti, Dimas |
| Ads Copywriter | Putri (leader), Fajar, Anisa, Yoga |
| Social Media | Dewi (leader), Raka, Nadia, Eko |
| Web QA | Hendra (leader), Laras, Andi, Maya |
| Developer Helper | Arief (leader), Citra, Bayu, Tari |
| Reporting | Kirana |
| Account Manager | Rizky |
| Finance | Melati |

Leader pakai model yang lebih pintar (Sonnet), anggota tim pakai model yang lebih murah (Haiku). Semua agent hanya membuat **draft**, tidak mengirim apa pun ke klien.

**Teknologi:** Node.js + Express (backend, Anthropic SDK), JavaScript biasa + Three.js (frontend), Vite (dev server dan build). Tanpa database.

## Daftar isi

1. [Persiapan](#1-persiapan)
2. [Install di Windows (lokal)](#2-install-di-windows-lokal)
3. [Install di MacBook (lokal)](#3-install-di-macbook-lokal)
4. [Install di VPS (domainkamu.com)](#4-install-di-vps-domainkamucom)
5. [Keamanan dan biaya](#5-keamanan-dan-biaya)
6. [Mengubah karyawan](#6-mengubah-karyawan)
7. [Struktur project](#7-struktur-project)
8. [Follow for support](#follow-for-support)

> Ganti `domainkamu.com` di seluruh panduan ini dengan domain milikmu sendiri.

---

## 1. Persiapan

**Yang dibutuhkan**

- **Node.js 20 atau lebih baru** (disarankan versi LTS terbaru).
- **Folder project** `kantor-ai` (hasil clone atau extract).
- **API key Anthropic** (opsional untuk tes tampilan, wajib untuk jawaban sungguhan). Buat di https://console.anthropic.com

**Mode demo vs mode live**

| Mode | Penjelasan |
|---|---|
| Demo | `ANTHROPIC_API_KEY` kosong. Jawaban berupa contoh, gratis. Cocok untuk tes tampilan. |
| Live | `ANTHROPIC_API_KEY` terisi. Jawaban sungguhan dari Claude, dikenai biaya API. |

**Variabel di file `.env`**

Salin `.env.example` menjadi `.env`, lalu isi sesuai kebutuhan.

| Variabel | Default | Fungsi |
|---|---|---|
| `ANTHROPIC_API_KEY` | (kosong) | API key. Kosong = mode demo. |
| `ACCESS_CODE` | (kosong) | Kode yang harus dimasukkan pengunjung. **Wajib diisi kalau situs publik.** |
| `PORT` | `3001` | Port server backend. |
| `MODEL_LEADER` | `claude-sonnet-5-5` | Model untuk leader divisi (lebih pintar). |
| `MODEL_TEAM` | `claude-haiku-4-5-20251001` | Model untuk anggota tim (lebih murah). |
| `MAX_TOKENS` | `1500` | Panjang maksimal jawaban. |
| `RATE_PER_MINUTE` | `15` | Batas request per IP per menit. |
| `DAILY_MAX_REQUESTS` | `300` | Batas total request per hari (jam Jakarta). |

**Soal `ACCESS_CODE`:** kode ini kamu bikin sendiri, bukan dapat dari mana-mana. Tulis kode apa saja setelah tanda `=`, misalnya `ACCESS_CODE=kantorku-2026-xyz`. Pakai kombinasi huruf, angka, dan tanda hubung, minimal 12 karakter. Untuk kode acak:

```bash
node -e "console.log(require('crypto').randomBytes(12).toString('hex'))"
```

> **Penting:** jangan pernah membagikan atau meng-commit file `.env`. Isinya API key pribadi. File ini sudah masuk `.gitignore`.

---

## 2. Install di Windows (lokal)

Cocok untuk mencoba atau mengembangkan di laptop Windows 10/11. Hasil akhirnya bisa dibuka di http://localhost:5173.

**Langkah 1: Install Node.js**

1. Buka https://nodejs.org, unduh installer versi LTS (Windows Installer `.msi`).
2. Jalankan installer, klik Next sampai selesai dengan pengaturan bawaan.
3. Buka PowerShell baru, lalu cek versinya:

```powershell
node -v
npm -v
```

Pastikan `node -v` menampilkan v20 atau lebih tinggi.

**Langkah 2: Ambil project**

Pakai Git:

```powershell
git clone https://github.com/Devanrifan/kantor-ai-agents.git
cd kantor-ai-agents
```

Atau extract file ZIP ke folder seperti `C:\Projects\kantor-ai`. Hindari folder yang dilindungi seperti Program Files.

**Langkah 3: Install dependency**

```powershell
npm install
```

**Langkah 4: Buat file `.env`**

```powershell
copy .env.example .env
notepad .env
```

Di Notepad, isi `ANTHROPIC_API_KEY` (kosongkan untuk mode demo), simpan, lalu tutup.

**Langkah 5: Jalankan**

```powershell
npm run dev
```

Buka browser ke http://localhost:5173. Untuk berhenti, tekan `Ctrl + C` di PowerShell.

**Kendala umum di Windows**

| Masalah | Solusi |
|---|---|
| `"npm" is not recognized` | Tutup dan buka ulang PowerShell. Kalau masih gagal, install ulang Node.js. |
| Script is disabled (execution policy) | Jalankan PowerShell sebagai Administrator: `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` |
| Port 5173 atau 3001 sudah dipakai | Tutup aplikasi lain yang memakai port itu, atau ubah `PORT` di `.env`. |
| Perubahan `.env` tidak terbaca | Hentikan (`Ctrl + C`) lalu jalankan `npm run dev` lagi. |

---

## 3. Install di MacBook (lokal)

Berlaku untuk MacBook Intel maupun Apple Silicon (M1/M2/M3/M4). Hasil akhirnya bisa dibuka di http://localhost:5173.

**Langkah 1: Install Node.js**

- **Opsi A (paling mudah):** unduh installer macOS (`.pkg`) versi LTS dari https://nodejs.org, lalu jalankan.
- **Opsi B (via Homebrew):** buka Terminal, lalu jalankan:

```bash
brew install node
```

Cek versinya, pastikan v20 atau lebih tinggi:

```bash
node -v
npm -v
```

**Langkah 2: Ambil project**

Pakai Git:

```bash
git clone https://github.com/Devanrifan/kantor-ai-agents.git
cd kantor-ai-agents
```

Atau klik dua kali file ZIP di Finder, lalu pindahkan foldernya ke lokasi yang mudah, misalnya `~/Projects/kantor-ai`.

**Langkah 3: Install dependency**

```bash
npm install
```

**Langkah 4: Buat file `.env`**

```bash
cp .env.example .env
nano .env
```

Isi `ANTHROPIC_API_KEY` (kosongkan untuk mode demo). Di nano: `Ctrl + O` lalu Enter untuk menyimpan, `Ctrl + X` untuk keluar.

**Langkah 5: Jalankan**

```bash
npm run dev
```

Buka Safari atau Chrome ke http://localhost:5173. Untuk berhenti, tekan `Control + C` di Terminal.

**Kendala umum di Mac**

| Masalah | Solusi |
|---|---|
| `command not found: brew` | Pakai Opsi A (installer `.pkg`), atau install Homebrew dari https://brew.sh |
| `EACCES` / permission denied saat `npm install` | Pastikan folder project ada di folder milikmu (bukan folder sistem). Jangan pakai `sudo` untuk `npm install`. |
| Port 5173 atau 3001 sudah dipakai | Tutup proses lain yang memakai port itu, atau ubah `PORT` di `.env`. |
| `.env` tidak kelihatan di Finder | Tekan `Command + Shift + titik`. |
| `code .` tidak dikenali | Di VS Code tekan `Cmd + Shift + P`, jalankan **Shell Command: Install 'code' command in PATH**. |

---

## 4. Install di VPS (domainkamu.com)

Frontend dan backend berjalan dari satu server Node. Contoh di bawah untuk Ubuntu 22.04 atau 24.04, dengan alamat akhir `https://office.domainkamu.com` (subdomain boleh diganti sesukamu).

> **Sebelum mulai:** siapkan akses SSH ke VPS, akses pengaturan DNS domain, dan API key Anthropic.

**Langkah 1: Arahkan DNS**

Di pengaturan DNS `domainkamu.com`, buat A record untuk subdomain `office` yang mengarah ke IP VPS. Tunggu beberapa menit sampai aktif (propagasi).

| Type | Name | Value |
|---|---|---|
| A | `office` | IP publik VPS kamu |

**Langkah 2: Install Node.js, nginx, dan pm2**

Masuk ke VPS lewat SSH, lalu jalankan:

```bash
sudo apt update && sudo apt install -y nginx git curl unzip
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2
node -v
```

**Langkah 3: Upload project**

Cara termudah, lewat Git:

```bash
sudo mkdir -p /var/www
sudo chown -R $USER:$USER /var/www
cd /var/www
git clone https://github.com/Devanrifan/kantor-ai-agents.git kantor-ai
```

Kalau repo private, VPS akan meminta login GitHub (pakai Personal Access Token sebagai password), atau gunakan SSH key.

Alternatif lewat ZIP, dari komputer lokal:

```bash
scp kantor-ai.zip user@IP_VPS:/tmp/
```

Lalu di VPS:

```bash
sudo mkdir -p /var/www
sudo unzip /tmp/kantor-ai.zip -d /var/www/
sudo rm -rf /var/www/__MACOSX
sudo chown -R $USER:$USER /var/www/kantor-ai
```

**Langkah 4: Install, build, dan isi `.env`**

```bash
cd /var/www/kantor-ai
npm install
npm run build
cp .env.example .env
nano .env
```

Isi minimal dua baris ini:

```
ANTHROPIC_API_KEY=sk-ant-xxxxxxxx
ACCESS_CODE=kode-rahasia-yang-susah-ditebak
```

> **Wajib:** `ACCESS_CODE` harus diisi sebelum situs bisa diakses publik. Tanpa itu, siapa pun yang tahu link bisa memakai API key kamu.

**Langkah 5: Jalankan dengan pm2**

```bash
pm2 start server/index.js --name kantor-ai
pm2 save
pm2 startup
```

Perintah `pm2 startup` akan mencetak satu baris perintah. Salin dan jalankan baris itu agar aplikasi otomatis hidup lagi setelah VPS restart. Cek statusnya:

```bash
pm2 status
pm2 logs kantor-ai
```

Di log seharusnya tampil `Mode: LIVE` bila API key sudah terbaca.

**Langkah 6: Konfigurasi nginx**

Buat file konfigurasi:

```bash
sudo nano /etc/nginx/sites-available/kantor-ai
```

Isi dengan:

```nginx
server {
  server_name office.domainkamu.com;

  location / {
    proxy_pass http://127.0.0.1:3001;
    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }

  # chat streaming: jangan di-buffer
  location /api/chat {
    proxy_pass http://127.0.0.1:3001;
    proxy_http_version 1.1;
    proxy_set_header Connection '';
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_buffering off;
    proxy_read_timeout 120s;
  }
}
```

Aktifkan dan muat ulang nginx:

```bash
sudo ln -s /etc/nginx/sites-available/kantor-ai /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

**Langkah 7: Pasang HTTPS (SSL gratis)**

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d office.domainkamu.com
```

Ikuti petunjuk di layar. Setelah selesai, buka `https://office.domainkamu.com` dan masukkan kode akses.

**Firewall (disarankan)**

```bash
sudo ufw allow OpenSSH
sudo ufw allow "Nginx Full"
sudo ufw enable
```

**Update di kemudian hari**

```bash
cd /var/www/kantor-ai
git pull && npm install && npm run build && pm2 restart kantor-ai
```

Kalau memakai ZIP, upload dan extract versi baru (jangan timpa file `.env`), lalu jalankan `npm install && npm run build && pm2 restart kantor-ai`.

**Kendala umum di VPS**

| Masalah | Solusi |
|---|---|
| 502 Bad Gateway | Aplikasi belum jalan. Cek `pm2 status` dan `pm2 logs kantor-ai`. |
| Halaman putih / file tidak ditemukan | Jalankan `npm run build` lagi, lalu `pm2 restart kantor-ai`. |
| Certbot gagal | Pastikan A record sudah mengarah ke IP VPS dan port 80/443 terbuka. |
| Jawaban chat muncul sekaligus, tidak mengalir | Pastikan blok `location /api/chat` di nginx memakai `proxy_buffering off`. |
| Mode masih DEMO | `ANTHROPIC_API_KEY` belum terbaca. Cek `.env` lalu `pm2 restart kantor-ai`. |

---

## 5. Keamanan dan biaya

- **Wajib isi `ACCESS_CODE`** kalau situs bisa diakses publik.
- API key hanya tersimpan di server (`.env`), tidak pernah dikirim ke browser.
- Pengaman bawaan: `RATE_PER_MINUTE` (batas per IP), `DAILY_MAX_REQUESTS` (batas total per hari, jam Jakarta), dan `MAX_TOKENS` (panjang jawaban).
- Pasang juga **batas pengeluaran bulanan** di Console Anthropic.
- Pemakaian token tiap chat tercatat di log (`pm2 logs kantor-ai`).
- Semua karyawan AI hanya membuat draft. Tidak ada yang mengirim apa pun ke klien.

---

## 6. Mengubah karyawan

| Yang ingin diubah | File / pengaturan |
|---|---|
| Nama, peran, keahlian, tampilan, tombol cepat | `shared/roster.js` |
| Aturan kerja umum dan prompt tiap divisi | `server/prompts.js` |
| Model yang dipakai | `MODEL_LEADER` dan `MODEL_TEAM` di `.env` |
| Isi ruangan (furniture) | `src/office.js` dan `src/furniture.js` |

Setelah mengubah file di atas: di lokal, server dev akan reload otomatis. Di VPS, jalankan `npm run build` lalu `pm2 restart kantor-ai`.

Link langsung ke satu karyawan: `https://office.domainkamu.com/#rina`

---

## 7. Struktur project

```
shared/roster.js   data karyawan & divisi (dipakai frontend dan backend)
server/            Express + Claude API (streaming, batas pemakaian)
src/               Three.js: scene, karakter, kamera, panel chat
```

---

## Follow for support

- Instagram: https://www.instagram.com/devanrifan/
- YouTube: https://www.youtube.com/@devanprogramming
- LinkedIn: https://id.linkedin.com/in/devanrifan
- Website: https://devanrifan.com/
- Tiktok: https://www.tiktok.com/@devanrifan