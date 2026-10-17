# Panduan OpenCode

Panduan lengkap penggunaan OpenCode: instalasi, pengaturan API key, integrasi VS Code dan Antigravity, cara pakai sehari-hari, konfigurasi lanjutan, dan troubleshooting.

Situs publik: https://htw-opencode.pages.dev/

## Struktur

```text
.
├── docs/
│   ├── index.md                  # Halaman utama
│   └── how-to-use-opencode.md    # Panduan lengkap
├── mkdocs.yml                    # Konfigurasi situs
├── requirements.txt              # Dependensi build
└── site/                         # Hasil build (tidak di-commit)
```

## Menjalankan Secara Lokal

```bash
python -m pip install -r requirements.txt
mkdocs serve
```

Buka http://127.0.0.1:8000

## Build Produksi

```bash
mkdocs build --strict
```

Hasil build ada di folder `site/`.

## Deploy

Situs di-deploy otomatis oleh Cloudflare Pages setiap kali ada push ke branch `main`.

- Build command: `python -m pip install -r requirements.txt && python -m mkdocs build --strict`
- Output directory: `site`
