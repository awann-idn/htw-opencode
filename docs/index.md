# Panduan OpenCode

Panduan lengkap untuk memasang, mengonfigurasi, dan memakai **OpenCode** — AI coding agent open-source yang berjalan di terminal, VS Code, dan Antigravity.

<div class="grid cards" markdown>

-   :material-download:{ .lg .middle } **Instalasi**

    ---

    Pasang OpenCode di Windows, macOS, atau Linux. Termasuk opsi WSL, npm, Homebrew, dan binary manual.

    [:octicons-arrow-right-24: Mulai instalasi](how-to-use-opencode.md#3-instalasi-opencode)

-   :material-key-variant:{ .lg .middle } **API Key**

    ---

    Dapatkan API key dari OpenCode Zen, Anthropic, OpenAI, Gemini, DeepSeek, atau OpenRouter. Tiga cara memasukkannya.

    [:octicons-arrow-right-24: Atur API key](how-to-use-opencode.md#5-memasukkan-api-key-ke-opencode)

-   :material-microsoft-visual-studio-code:{ .lg .middle } **VS Code**

    ---

    Integrasi ekstensi resmi: auto-install, instalasi manual, VSIX, dan shortcut yang wajib diketahui.

    [:octicons-arrow-right-24: Integrasi VS Code](how-to-use-opencode.md#7-integrasi-ke-visual-studio-code)

-   :material-google:{ .lg .middle } **Antigravity**

    ---

    Pakai OpenCode lewat terminal terintegrasi atau ekstensi di IDE Antigravity berbasis VS Code.

    [:octicons-arrow-right-24: Integrasi Antigravity](how-to-use-opencode.md#8-integrasi-ke-antigravity)

-   :material-console:{ .lg .middle } **Cara Pakai**

    ---

    Mode Plan vs Build, referensi file dengan `@`, undo/redo, perintah TUI, dan tips prompt yang efektif.

    [:octicons-arrow-right-24: Panduan harian](how-to-use-opencode.md#9-cara-pakai-sehari-hari)

-   :material-wrench:{ .lg .middle } **Troubleshooting**

    ---

    Solusi error umum: command tidak dikenali, ekstensi gagal, `ProviderInitError`, model tidak ditemukan, dan lainnya.

    [:octicons-arrow-right-24: Lihat solusi](how-to-use-opencode.md#11-troubleshooting)

</div>

## Quick Start

```bash
# 1. Install (butuh Node.js 18+)
npm install -g opencode-ai

# 2. Masuk ke folder project
cd path/ke/project

# 3. Jalankan
opencode

# 4. Di dalam TUI: sambungkan provider dan pilih model
#    /connect  -> pilih provider -> paste API key
#    /models   -> pilih model

# 5. Inisialisasi project (membuat AGENTS.md)
#    /init
```

!!! tip "Baru pertama kali?"
    Mulai dari [bagian instalasi](how-to-use-opencode.md#3-instalasi-opencode), lalu ikuti urutan sampai [menjalankan OpenCode pertama kali](how-to-use-opencode.md#6-menjalankan-opencode-pertama-kali). Total waktu setup sekitar 10–15 menit.

!!! note "OpenCode v1 vs v2"
    Panduan ini ditulis untuk OpenCode v1.19.x. OpenCode v2 juga tersedia dan memakai perintah `opencode` yang sama, sehingga keduanya tidak bisa dipasang berdampingan dengan nama perintah default. Dokumentasi v2 ada di [opencode.ai/v2/docs](https://opencode.ai/v2/docs).
