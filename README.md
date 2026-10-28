# AM Magic Link — Alight Motion Magic Link API UI

Frontend HTML/CSS/JavaScript dengan Vercel Functions sebagai proxy server agar API key tidak pernah dikirim ke browser.

## Struktur

- `public/index.html` — UI responsif.
- `api/send-magiclink.js` — proxy POST untuk send magic link.
- `api/verify-account.js` — proxy POST untuk verify account.
- `api/apply-premium.js` — proxy POST untuk apply premium.
- `api/_proxy.js` — validasi payload, autentikasi upstream, timeout, dan penanganan respons.

## Konfigurasi di Vercel

Buka **Vercel → Project → Settings → Environment Variables**, lalu tambahkan:

| Variable | Isi |
|---|---|
| `SEND_MAGICLINK_URL` | URL upstream endpoint send-magiclink |
| `VERIFY_ACCOUNT_URL` | URL upstream endpoint verify-account |
| `APPLY_PREMIUM_URL` | URL upstream endpoint apply-premium |
| `API_KEY` | API key rahasia dari penyedia API |
| `API_KEY_HEADER` | Opsional; default `x-api-key` |
| `API_KEY_PREFIX` | Opsional; kosong untuk API key biasa, atau `Bearer ` bila API mengharuskannya |

Jangan menambahkan tanda kutip kecuali memang bagian dari nilai. Sesuaikan header dengan dokumentasi API penyedia. Contoh: jika API meminta `Authorization: Bearer <key>`, set `API_KEY_HEADER=Authorization` dan `API_KEY_PREFIX=Bearer `.

Setelah mengubah environment variables, lakukan redeploy.

## Deploy dari HP Android

1. Buat repository GitHub baru dan upload seluruh isi folder proyek ini.
2. Di Vercel, pilih **Add New → Project**, lalu impor repository tersebut.
3. Tambahkan environment variables seperti tabel di atas sebelum deploy.
4. Deploy. Halaman utama disajikan dari `public/index.html` dan fungsi tersedia di `/api/...`.

## Kontrak request

Setiap endpoint frontend mengirim `POST /api/<nama-endpoint>` dengan `Content-Type: application/json`. Proxy mengirim body JSON itu ke URL upstream terkait dan menambahkan API key di header yang ditentukan melalui environment variables.

Payload pada UI hanyalah contoh. **Field dan format request harus disesuaikan dengan dokumentasi API asli.** Proxy saat ini menggunakan metode POST untuk semua endpoint; ubah jika penyedia API mensyaratkan metode atau skema berbeda.

## Catatan keamanan

- Jangan commit `.env` atau API key ke GitHub.
- `.env.example` hanya berisi contoh placeholder, bukan rahasia aktif.
- Gunakan HTTPS untuk API upstream publik.
- Untuk penggunaan publik, pertimbangkan rate limiting, autentikasi pengguna, logging yang tidak menyimpan data rahasia, dan perlindungan penyalahgunaan endpoint.
- Endpoint `apply-premium` hanya sesuai untuk integrasi yang sah dan diotorisasi oleh pemilik layanan/API.
