## PREPARATION
### Backend
- npm install (install dependency)
- npm run migrate (setup database mongodb & mysql if not exists)
- npm run drop (drop database)
- npm run fresh (drop and recreate database for clean data)
- npm run dev (run deploy development)
- npm run start (run deploy)

### frontend
- npm install (install dependency)
- npm run dev (run deploy development)

## FASE 1 WAF (Nginx + ModSecurity)
- Semua trafik publik masuk ke `nginx` (port `80`).
- `backend` dan `frontend` tidak lagi dipublish langsung ke host, hanya `expose` internal Docker network.
- ModSecurity dijalankan pada Nginx dengan mode awal `DetectionOnly` agar tidak langsung memblok request legit.

### Komponen Baru
- `nginx/Dockerfile`: image Nginx + modul ModSecurity + OWASP CRS.
- `nginx/conf.d/default.conf`: reverse proxy route ke `frontend:3000` dan `backend:5000` (`/api/*`).
- `nginx/modsec/main.conf`: aktivasi ModSecurity + CRS dengan `SecRuleEngine DetectionOnly`.

### Jalankan Stack
1. Siapkan env file:
   - salin `backend/.env.example` menjadi `backend/.env`
   - salin `frontend/.env.example` menjadi `frontend/.env`
2. Build dan jalankan:
   - `docker compose up -d --build`
3. Akses aplikasi lewat:
   - `http://localhost`

### Catatan Operasional
- Log akses Nginx: `/var/log/nginx/access.log`
- Log error Nginx: `/var/log/nginx/error.log`
- Audit log ModSecurity: `/var/log/nginx/modsec_audit.log`
- Setelah tuning false positive, ubah `SecRuleEngine DetectionOnly` menjadi `SecRuleEngine On` di `nginx/modsec/main.conf`.


