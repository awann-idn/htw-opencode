# Panduan Lengkap Menggunakan OpenCode

> Panduan step-by-step: instalasi, memasukkan API key, integrasi ke VS Code & Antigravity, sampai pemakaian sehari-hari.
>
> Diuji pada OpenCode v1.19.x (Windows). OpenCode v2 juga sudah tersedia — keduanya memakai perintah `opencode` yang sama dan **tidak bisa dipasang berdampingan** dengan nama perintah default.

---

## Daftar Isi

1. [Apa itu OpenCode?](#1-apa-itu-opencode)
2. [Prasyarat](#2-prasyarat)
3. [Instalasi OpenCode](#3-instalasi-opencode)
4. [Mendapatkan API Key](#4-mendapatkan-api-key)
5. [Memasukkan API Key ke OpenCode](#5-memasukkan-api-key-ke-opencode)
6. [Menjalankan OpenCode Pertama Kali](#6-menjalankan-opencode-pertama-kali)
7. [Integrasi ke Visual Studio Code](#7-integrasi-ke-visual-studio-code)
8. [Integrasi ke Antigravity](#8-integrasi-ke-antigravity)
9. [Cara Pakai Sehari-hari](#9-cara-pakai-sehari-hari)
10. [Konfigurasi Lanjutan](#10-konfigurasi-lanjutan)
11. [Troubleshooting](#11-troubleshooting)
12. [Cheat Sheet](#12-cheat-sheet)

---

## 1. Apa itu OpenCode?

OpenCode adalah **AI coding agent open-source** yang berjalan di terminal (TUI), desktop app, atau sebagai ekstensi IDE. OpenCode bisa:

- Membaca dan menjelaskan isi codebase
- Menulis, mengubah, dan me-refactor kode
- Menjalankan perintah terminal
- Membuat rencana implementasi sebelum coding
- Terhubung ke 75+ provider LLM (Anthropic, OpenAI, Google, DeepSeek, OpenRouter, dll.)

Ada tiga cara memakai OpenCode:

| Mode | Cara Pakai | Cocok Untuk |
|------|-----------|-------------|
| **TUI** | Jalankan `opencode` di terminal | Pemakaian utama, paling lengkap |
| **IDE Extension** | Otomatis terpasang saat `opencode` dijalankan di terminal VS Code/Antigravity | Yang nyaman kerja di editor |
| **CLI non-interaktif** | `opencode run "perintah"` | Otomasi / script |

---

## 2. Prasyarat

Sebelum mulai, siapkan:

1. **Terminal modern**
   - Windows: Windows Terminal, PowerShell 7, atau WSL
   - macOS/Linux: WezTerm, Alacritty, Ghostty, Kitty, atau terminal bawaan

2. **Node.js 18+** (kalau mau install lewat npm)
   - Cek: `node -v` dan `npm -v`
   - Download: https://nodejs.org

3. **API key LLM provider**
   - Bisa dari OpenCode Zen (paling gampang), Anthropic, OpenAI, Google, DeepSeek, OpenRouter, Groq, dll.
   - Detail di [bagian 4](#4-mendapatkan-api-key).

4. **Git** (disarankan)
   - OpenCode memakai Git untuk fitur undo/redo dan snapshot perubahan.
   - Cek: `git --version`

> **Khusus Windows:** OpenCode tetap jalan native di Windows, tapi tim OpenCode merekomendasikan **WSL** untuk performa dan kompatibilitas terbaik. Kalau kamu pakai WSL, install OpenCode di dalam WSL (bukan di PowerShell), lalu buka project lewat `/mnt/c/...` atau clone ulang di home WSL.

---

## 3. Instalasi OpenCode

Pilih **satu** metode saja. Jangan campur beberapa metode karena bisa bentrok.

### 3.1 Windows

#### Opsi A — npm (paling umum)

```powershell
npm install -g opencode-ai
```

#### Opsi B — Chocolatey

```powershell
choco install opencode
```

#### Opsi C — Scoop

```powershell
scoop install opencode
```

#### Opsi D — WSL (direkomendasikan untuk pengalaman terbaik)

Masuk ke WSL dulu:

```powershell
wsl
```

Lalu jalankan installer Linux:

```bash
curl -fsSL https://opencode.ai/install | bash
```

#### Opsi E — Binary manual

1. Buka https://github.com/anomalyco/opencode/releases
2. Download file `opencode-windows-x64.zip` (atau `-baseline` kalau CPU lama)
3. Extract ke folder, misalnya `C:\Tools\opencode`
4. Tambahkan folder itu ke **PATH**:
   - `Win + R` → ketik `sysdm.cpl` → **Advanced** → **Environment Variables**
   - Edit `Path` → **New** → `C:\Tools\opencode` → OK
5. Tutup dan buka ulang terminal

### 3.2 macOS / Linux

#### Installer script (paling gampang)

```bash
curl -fsSL https://opencode.ai/install | bash
```

#### Homebrew (macOS/Linux)

```bash
brew install anomalyco/tap/opencode
```

#### Arch Linux

```bash
sudo pacman -S opencode
# atau versi terbaru dari AUR:
paru -S opencode-bin
```

#### npm / pnpm / bun / yarn

```bash
npm install -g opencode-ai
pnpm install -g opencode-ai
bun install -g opencode-ai
yarn global add opencode-ai
```

#### Docker

```bash
docker run -it --rm ghcr.io/anomalyco/opencode
```

### 3.3 Verifikasi Instalasi

Tutup terminal, buka lagi, lalu cek:

```bash
opencode --version
```

Kalau muncul nomor versi (contoh `1.19.3`), instalasi berhasil.

Kalau muncul `'opencode' is not recognized`:

- Pastikan terminal sudah di-restart
- Cek lokasi binary:
  - Windows: `where opencode`
  - macOS/Linux: `which opencode`
- Kalau npm global bin belum masuk PATH, jalankan `npm config get prefix` lalu tambahkan folder itu ke PATH.

### 3.4 Update & Uninstall

```bash
opencode upgrade     # update ke versi terbaru
opencode uninstall   # hapus OpenCode
```

---

## 4. Mendapatkan API Key

OpenCode **tidak menyediakan model secara gratis** (kecuali kamu punya subscription tertentu). Kamu butuh API key dari provider.

### 4.1 OpenCode Zen (rekomendasi untuk pemula)

OpenCode Zen adalah kumpulan model yang sudah dites dan diverifikasi tim OpenCode.

1. Buka https://opencode.ai/auth
2. Login / daftar
3. Tambahkan metode pembayaran
4. Copy API key yang muncul

Kelebihan: satu key untuk banyak model, setup paling simpel.

### 4.2 Provider populer lain

| Provider | Link API Key | Model Contoh | Catatan |
|----------|-------------|--------------|---------|
| **Anthropic** | https://console.anthropic.com | Claude Sonnet, Claude Opus | Bisa login Claude Pro/Max lewat OAuth |
| **OpenAI** | https://platform.openai.com/api-keys | GPT-4.1, GPT-5 | Butuh saldo |
| **Google (Gemini)** | https://aistudio.google.com/apikey | Gemini 2.5 Pro/Flash | Ada tier gratis terbatas |
| **DeepSeek** | https://platform.deepseek.com | DeepSeek V4 | Murah |
| **OpenRouter** | https://openrouter.ai/keys | Banyak model | Satu key untuk banyak provider |
| **Groq** | https://console.groq.com | Llama, Qwen | Sangat cepat |
| **GitHub Copilot** | https://github.com/features/copilot | GPT/Claude via Copilot | Kalau punya subscription Copilot |

> **Tips aman:** Jangan pernah commit API key ke Git. Simpan di password manager, dan jangan screenshot key secara utuh.

---

## 5. Memasukkan API Key ke OpenCode

Ada **tiga cara**. Pilih salah satu — cara pertama paling gampang.

### 5.1 Cara A — `/connect` (paling mudah, direkomendasikan)

1. Jalankan OpenCode di folder project:

   ```bash
   cd /path/ke/project
   opencode
   ```

2. Di dalam TUI, ketik:

   ```
   /connect
   ```

3. Pilih provider:
   - **OpenCode Zen** → browser terbuka ke halaman login → copy key
   - **Anthropic** → pilih *Manually enter API Key* atau login Claude Pro/Max
   - **OpenAI**, **DeepSeek**, **OpenRouter**, dll. → paste key

4. Paste API key, tekan **Enter**.

5. Cek model yang tersedia:

   ```
   /models
   ```

6. Pilih model, tekan **Enter**.

Key disimpan otomatis di:

| OS | Lokasi |
|----|--------|
| Windows | `%USERPROFILE%\.local\share\opencode\auth.json` |
| macOS/Linux | `~/.local/share/opencode/auth.json` |

File ini **jangan** di-commit ke repository.

### 5.2 Cara B — Environment Variable

Cocok kalau kamu tidak mau key tersimpan di `auth.json`, atau untuk CI/CD.

**Windows (PowerShell) — permanen:**

```powershell
setx ANTHROPIC_API_KEY "sk-ant-xxxxxxxx"
```

Tutup dan buka ulang terminal, lalu cek:

```powershell
echo $env:ANTHROPIC_API_KEY
```

**Windows (CMD):**

```cmd
setx ANTHROPIC_API_KEY "sk-ant-xxxxxxxx"
```

**macOS/Linux (bash/zsh):**

Tambahkan ke `~/.bashrc`, `~/.zshrc`, atau `~/.profile`:

```bash
export ANTHROPIC_API_KEY="sk-ant-xxxxxxxx"
export OPENAI_API_KEY="sk-xxxxxxxx"
export DEEPSEEK_API_KEY="sk-xxxxxxxx"
export OPENROUTER_API_KEY="sk-or-xxxxxxxx"
export GEMINI_API_KEY="AIza-xxxxxxxx"
```

Lalu reload:

```bash
source ~/.zshrc
```

OpenCode otomatis mendeteksi environment variable provider yang umum. Kalau tidak terdeteksi, pakai Cara A atau Cara C.

### 5.3 Cara C — Config File `opencode.json`

Cocok untuk konfigurasi yang rapi dan bisa dibedakan per project.

Buat file `opencode.json` di root project (atau global di `~/.config/opencode/opencode.json`):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    }
  }
}
```

Dengan cara ini, key tetap di environment variable, bukan di file config. Kalau mau membaca key dari file terpisah:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openai": {
      "options": {
        "apiKey": "{file:~/.secrets/openai-key}"
      }
    }
  }
}
```

Lalu isi `~/.secrets/openai-key` hanya dengan key-nya saja.

> **Penting:** `opencode.json` yang berisi key mentah jangan di-commit. Kalau perlu di-commit, gunakan `{env:...}` atau `{file:...}`.

### 5.4 Cek Key yang Sudah Masuk

Dari terminal:

```bash
opencode auth list
```

Dari dalam TUI:

```
/models
```

Kalau provider muncul dan model bisa dipilih, berarti key sudah benar.

---

## 6. Menjalankan OpenCode Pertama Kali

### 6.1 Masuk ke Folder Project

```bash
cd D:\WEBSITE\AI\project-kamu
```

Kalau project belum ada, buat folder kosong dulu:

```bash
mkdir project-baru
cd project-baru
```

### 6.2 Jalankan OpenCode

```bash
opencode
```

Tunggu TUI terbuka. Di sudut kanan bawah akan terlihat mode aktif (biasanya **Build**).

### 6.3 Inisialisasi Project

Di dalam TUI, ketik:

```
/init
```

OpenCode akan menganalisis project dan membuat file `AGENTS.md` di root project. File ini berisi ringkasan struktur project, teknologi yang dipakai, dan aturan coding. Commit file ini ke Git supaya OpenCode berikutnya langsung paham konteks project.

### 6.4 Tes Cepat

Coba tanya sesuatu:

```
Jelaskan struktur project ini dan teknologi apa saja yang dipakai.
```

Atau minta perubahan kecil:

```
Tambahkan komentar dokumentasi pada fungsi utama di file src/index.ts
```

---

## 7. Integrasi ke Visual Studio Code

OpenCode punya ekstensi resmi untuk VS Code dan fork-fork-nya (Cursor, Windsurf, VSCodium). Integrasi ini membuat OpenCode bisa:

- Membuka terminal terpisah di dalam VS Code
- Otomatis tahu file/tab yang sedang kamu buka
- Insert referensi file dengan shortcut
- Share seleksi kode ke OpenCode

### 7.1 Instalasi Otomatis (paling gampang)

1. Buka VS Code
2. Buka folder project: **File → Open Folder**
3. Buka terminal terintegrasi: **Ctrl + `** (tombol backtick)
4. Jalankan:

   ```bash
   opencode
   ```

5. Ekstensi OpenCode akan **terpasang otomatis** saat pertama kali dijalankan.

> Syarat: perintah `code` harus tersedia di PATH. Kalau belum:
> - `Ctrl + Shift + P` → cari **Shell Command: Install 'code' command in PATH** → Enter
> - Tutup dan buka ulang terminal

### 7.2 Instalasi Manual

1. Buka **Extensions** (`Ctrl + Shift + X`)
2. Cari **OpenCode**
3. Pilih yang publisher-nya **sst-dev** (`sst-dev.opencode`)
4. Klik **Install**
5. Reload VS Code kalau diminta

Alternatif lewat terminal:

```bash
code --install-extension sst-dev.opencode
```

Atau download VSIX dari Open VSX:

- https://open-vsx.org/extension/sst-dev/opencode
- Di VS Code: `Ctrl + Shift + P` → **Extensions: Install from VSIX...** → pilih file `.vsix`

### 7.3 Shortcut Penting di VS Code

| Shortcut (Windows/Linux) | Shortcut (macOS) | Fungsi |
|--------------------------|------------------|--------|
| `Ctrl + Esc` | `Cmd + Esc` | Buka OpenCode di split terminal / fokus ke sesi yang ada |
| `Ctrl + Shift + Esc` | `Cmd + Shift + Esc` | Mulai sesi OpenCode baru |
| `Ctrl + Alt + K` | `Cmd + Option + K` | Insert referensi file (contoh: `@File#L37-42`) |

Seleksi kode yang sedang aktif otomatis dibagikan ke OpenCode sebagai konteks.

### 7.4 Mengatur Editor untuk `/editor` dan `/export`

Kalau kamu memakai perintah `/editor` atau `/export` dari TUI dan ingin file terbuka di VS Code:

**Windows (PowerShell):**

```powershell
setx EDITOR "code --wait"
```

**macOS/Linux:**

```bash
export EDITOR="code --wait"
```

Tambahkan ke `~/.zshrc` / `~/.bashrc` supaya permanen.

---

## 8. Integrasi ke Antigravity

Antigravity adalah IDE berbasis VS Code dari Google. Karena itu, OpenCode bisa dipakai dengan **dua cara**:

1. **Lewat terminal terintegrasi** (selalu berfungsi, tanpa ekstensi)
2. **Lewat ekstensi OpenCode** (kalau marketplace Antigravity mendukung)

> Catatan: Antigravity punya agent sendiri (Gemini). OpenCode adalah agent terpisah. Keduanya bisa hidup berdampingan — kamu bebas memilih mau pakai yang mana di project yang sama.

### 8.1 Cara 1 — Terminal Terintegrasi (paling aman)

1. Install **Antigravity IDE** dari https://antigravity.google/download
2. Buka folder project: **File → Open Folder**
3. Buka terminal: **Ctrl + `** (tombol backtick)
4. Jalankan:

   ```bash
   opencode
   ```

5. OpenCode langsung berjalan di dalam Antigravity. Semua fitur TUI tersedia.

Kelemahan cara ini: OpenCode tidak otomatis tahu file/tab yang sedang aktif (kecuali ekstensi terpasang).

### 8.2 Cara 2 — Ekstensi OpenCode

1. Buka **Extensions** di Antigravity (`Ctrl + Shift + X`)
2. Cari **OpenCode**
3. Kalau muncul, pilih publisher **sst-dev** → **Install**
4. Kalau tidak muncul di marketplace Antigravity, install manual lewat VSIX:
   - Download: https://open-vsx.org/extension/sst-dev/opencode
   - Atau link langsung: https://open-vsx.org/api/sst-dev/opencode/1.0.0/file/sst-dev.opencode-1.0.0.vsix
   - Di Antigravity: `Ctrl + Shift + P` → **Extensions: Install from VSIX...** → pilih file `.vsix`
5. Reload IDE kalau diminta
6. Jalankan `opencode` di terminal terintegrasi

Kalau ekstensi gagal terpasang otomatis, cek:

- Apakah perintah CLI IDE tersedia di PATH (misalnya `antigravity`, `code`, atau nama lain sesuai instalasi)
- Apakah Antigravity punya izin memasang ekstensi
- Coba jalankan `opencode` dari terminal **di dalam** Antigravity, bukan dari terminal luar

### 8.3 Shortcut di Antigravity

Karena berbasis VS Code, shortcut biasanya sama:

| Shortcut | Fungsi |
|----------|--------|
| `Ctrl + Esc` | Buka / fokus OpenCode |
| `Ctrl + Shift + Esc` | Sesi OpenCode baru |
| `Ctrl + Alt + K` | Insert referensi file |

Kalau shortcut bentrok dengan shortcut Antigravity, ubah di **Keyboard Shortcuts** (`Ctrl + K` lalu `Ctrl + S`).

### 8.4 Rekomendasi Pemakaian

- Pakai **terminal terintegrasi** kalau hanya butuh OpenCode sesekali.
- Pakai **ekstensi** kalau sering kerja di editor dan ingin konteks file otomatis.
- Jangan jalankan OpenCode dan agent Antigravity pada task yang sama secara bersamaan — bisa saling menimpa perubahan file.

---

## 9. Cara Pakai Sehari-hari

### 9.1 Dua Mode Utama: Plan vs Build

Tekan **Tab** untuk berpindah mode.

| Mode | Fungsi |
|------|--------|
| **Plan** | OpenCode hanya membuat rencana, **tidak mengubah file** |
| **Build** | OpenCode boleh mengubah file dan menjalankan perintah |

Alur yang disarankan untuk fitur besar:

1. Masuk **Plan** mode
2. Jelaskan kebutuhan sedetail mungkin
3. Review rencananya, kasih feedback
4. Kalau sudah oke, tekan **Tab** ke **Build**
5. Minta: `Gas, implementasikan rencananya.`

### 9.2 Memberi Konteks File

Gunakan `@` untuk mencari file:

```
Jelaskan cara autentikasi di @src/api/auth.ts
```

Atau variasinya:

```
Implementasi validasi yang sama seperti di @src/utils/validate.ts ke @src/api/user.ts
```

Di VS Code/Antigravity, gunakan `Ctrl + Alt + K` untuk insert referensi file otomatis.

### 9.3 Undo & Redo

```
/undo
```

Membatalkan perubahan terakhir dan mengembalikan prompt sebelumnya. Bisa dijalankan berkali-kali.

```
/redo
```

Mengulangi perubahan yang sudah di-undo.

### 9.4 Perintah TUI yang Sering Dipakai

| Perintah | Fungsi |
|----------|--------|
| `/init` | Analisis project, buat `AGENTS.md` |
| `/connect` | Tambah / ganti API key provider |
| `/models` | Pilih model LLM |
| `/undo` | Batalkan perubahan terakhir |
| `/redo` | Ulangi perubahan yang dibatalkan |
| `/compact` | Ringkas konteks percakapan (hemat token) |
| `/editor` | Buka prompt di editor eksternal |
| `/export` | Export percakapan |
| `/help` | Bantuan |
| `/exit` | Keluar dari OpenCode |

### 9.5 CLI Non-Interaktif

Untuk otomasi atau sekali jalan:

```bash
opencode run "Jelaskan fungsi package.json di project ini"
```

```bash
opencode run "Buat unit test untuk src/utils/math.ts"
```

Cek daftar model yang tersedia:

```bash
opencode models
```

### 9.6 Tips Prompt yang Efektif

- **Spesifik**: sebutkan file, fungsi, dan hasil yang diinginkan.
- **Beri contoh**: tunjukkan pola yang sudah ada di project.
- **Satu task per prompt**: jangan campur 5 fitur dalam satu pesan.
- **Minta rencana dulu** untuk perubahan besar.
- **Gunakan `AGENTS.md`** untuk menulis aturan project (contoh: "selalu pakai TypeScript strict", "test pakai Vitest").
- **Drag & drop gambar** ke terminal kalau perlu referensi desain.

---

## 10. Konfigurasi Lanjutan

### 10.1 Lokasi Config

| Jenis | Windows | macOS/Linux |
|-------|---------|-------------|
| Global | `%USERPROFILE%\.config\opencode\opencode.json` | `~/.config/opencode/opencode.json` |
| Project | `<project>\opencode.json` | `<project>/opencode.json` |
| Auth | `%USERPROFILE%\.local\share\opencode\auth.json` | `~/.local/share/opencode/auth.json` |
| Log | `%USERPROFILE%\.local\share\opencode\log` | `~/.local/share/opencode/log` |

Config digabung, bukan saling menimpa. Project config mengalahkan global config untuk key yang sama.

### 10.2 Contoh Config Lengkap

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5",
  "autoupdate": true,
  "permission": {
    "edit": "ask",
    "bash": "ask"
  },
  "formatter": true,
  "lsp": true,
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md"],
  "provider": {
    "anthropic": {
      "options": {
        "timeout": 600000
      }
    }
  }
}
```

Penjelasan singkat:

- `model` — model utama
- `small_model` — model murah untuk task ringan (judul sesi, dll.)
- `permission` — minta izin sebelum edit file / jalankan bash
- `formatter` / `lsp` — aktifkan formatter dan language server
- `instructions` — file aturan tambahan yang selalu dibaca

### 10.3 Keamanan

Sangat disarankan untuk project yang sensitif:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "ask",
    "bash": "ask",
    "webfetch": "ask"
  }
}
```

Tambahkan ke `.gitignore`:

```gitignore
# Jangan commit kredensial
auth.json
.secrets/
.env
.env.*
```

Dan **jangan** taruh API key mentah di `opencode.json` yang di-commit. Gunakan `{env:NAMA_VAR}` atau `{file:path/ke/key}`.

### 10.4 Model Reference

Format referensi model selalu:

```
<provider>/<model>
```

Contoh:

```
anthropic/claude-sonnet-4-5
openai/gpt-4.1
openrouter/google/gemini-2.5-flash
deepseek/deepseek-chat
```

Cek yang tersedia dengan:

```bash
opencode models
```

---

## 11. Troubleshooting

### 11.1 `opencode` tidak dikenali

**Gejala:** `'opencode' is not recognized as an internal or external command`

**Solusi:**

1. Tutup dan buka ulang terminal
2. Cek lokasi: `where opencode` (Windows) atau `which opencode` (macOS/Linux)
3. Kalau tidak ada, install ulang
4. Kalau ada tapi tidak terdeteksi, tambahkan folder binary ke PATH

### 11.2 Ekstensi VS Code / Antigravity tidak terpasang otomatis

**Solusi:**

1. Pastikan `opencode` dijalankan dari **terminal terintegrasi** IDE
2. Pastikan CLI IDE ada di PATH:
   - VS Code: `code --version`
   - Antigravity: cek perintah CLI sesuai instalasi
3. Install manual dari marketplace atau VSIX (lihat bagian 7.2 dan 8.2)
4. Restart IDE

### 11.3 `ProviderInitError`

**Penyebab:** config rusak atau auth korup.

**Solusi:**

1. Tutup OpenCode
2. Hapus folder data:
   - Windows: `%USERPROFILE%\.local\share\opencode`
   - macOS/Linux: `~/.local/share/opencode`
3. Buka OpenCode lagi
4. Jalankan `/connect` ulang

> Menghapus folder ini juga menghapus riwayat sesi. Backup dulu kalau perlu.

### 11.4 `ProviderModelNotFoundError`

**Penyebab:** nama model salah.

**Solusi:**

1. Cek daftar model: `opencode models`
2. Pastikan formatnya `<provider>/<model>`
3. Pastikan provider sudah terautentikasi: `opencode auth list`
4. Cek ejaan — case-sensitive

### 11.5 Error 401 / 403 dari provider

**Penyebab:** API key salah, expired, atau tidak punya akses model.

**Solusi:**

1. Cek key di dashboard provider
2. Jalankan ulang `/connect`
3. Pastikan billing/saldo provider aktif
4. Pastikan model yang dipilih memang tersedia untuk akun kamu

### 11.6 Performa lambat di Windows

**Solusi:**

- Gunakan **WSL** dan jalankan OpenCode di dalam WSL
- Hindari membuka project dari drive jaringan
- Matikan snapshot untuk repo besar:

  ```json
  {
    "snapshot": false
  }
  ```

  Catatan: undo/redo berbasis snapshot tidak akan berfungsi.

### 11.7 Error package provider (`AI_APICallError`)

**Solusi:**

1. Tutup OpenCode
2. Hapus cache:
   - Windows: `%USERPROFILE%\.cache\opencode`
   - macOS/Linux: `~/.cache/opencode`
3. Jalankan OpenCode lagi — package provider akan di-download ulang

### 11.8 Melihat Log

- Windows: `Win + R` → `%USERPROFILE%\.local\share\opencode\log`
- macOS/Linux: `~/.local/share/opencode/log`

Atau jalankan dengan log detail:

```bash
opencode --log-level DEBUG
```

### 11.9 Copy-paste tidak jalan di Linux

```bash
# X11
sudo apt install -y xclip
# atau
sudo apt install -y xsel

# Wayland
sudo apt install -y wl-clipboard
```

---

## 12. Cheat Sheet

### Instalasi

```bash
npm install -g opencode-ai          # semua OS (butuh Node.js)
curl -fsSL https://opencode.ai/install | bash   # macOS/Linux/WSL
brew install anomalyco/tap/opencode # macOS/Linux
choco install opencode              # Windows
scoop install opencode              # Windows
```

### Verifikasi & Maintenance

```bash
opencode --version
opencode upgrade
opencode uninstall
opencode models
opencode auth list
```

### Menjalankan

```bash
opencode                    # TUI di folder saat ini
opencode run "prompt"       # sekali jalan, non-interaktif
opencode --log-level DEBUG  # debug
```

### Di Dalam TUI

```
/init       analisis project + buat AGENTS.md
/connect    tambah API key
/models     pilih model
/undo       batalkan perubahan
/redo       ulangi perubahan
/compact    ringkas konteks
/editor     buka prompt di editor eksternal
/export     export percakapan
/help       bantuan
/exit       keluar
Tab         pindah Plan <-> Build
@nama_file  referensi file
```

### Shortcut IDE

```
Ctrl + Esc          buka / fokus OpenCode
Ctrl + Shift + Esc  sesi baru
Ctrl + Alt + K      insert referensi file
```

### Lokasi Penting (Windows)

```
%USERPROFILE%\.config\opencode\opencode.json          config global
%USERPROFILE%\.local\share\opencode\auth.json         API key
%USERPROFILE%\.local\share\opencode\log               log
%USERPROFILE%\.cache\opencode                         cache provider
```

### Lokasi Penting (macOS/Linux)

```
~/.config/opencode/opencode.json      config global
~/.local/share/opencode/auth.json     API key
~/.local/share/opencode/log           log
~/.cache/opencode                     cache provider
```

---

## Link Berguna

- Dokumentasi resmi: https://opencode.ai/docs
- Dokumentasi v2: https://opencode.ai/v2/docs
- Providers: https://opencode.ai/docs/providers
- IDE extension: https://opencode.ai/docs/ide
- Config: https://opencode.ai/docs/config
- Troubleshooting: https://opencode.ai/docs/troubleshooting
- GitHub: https://github.com/anomalyco/opencode
- Discord: https://opencode.ai/discord
- Open VSX (VSIX): https://open-vsx.org/extension/sst-dev/opencode

---

**Selamat coding!** Mulai dari `opencode` di folder project, jalankan `/init`, lalu ngobrol seperti kamu ngobrol dengan developer junior — makin jelas konteks yang kamu kasih, makin bagus hasilnya.
