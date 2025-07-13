## Dockerfile for Front End

```sql
# Stage 1: Build the Next.js app
FROM node:20-alpine AS builder

WORKDIR /app

# Copy dependency definitions
COPY package*.json ./
COPY tsconfig.json ./
COPY next.config.ts ./
COPY postcss.config.mjs ./
COPY tailwind.config.ts ./
COPY eslint.config.mjs ./
COPY components.json ./

# Install dependencies
RUN npm install

# Copy all sources
COPY . .

# Build the Next.js app (output: .next/)
RUN npm run build

# Stage 2: Production image, serving static files with Next.js
FROM node:20-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Copy only necessary files from builder
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/next.config.ts ./
COPY --from=builder /app/tailwind.config.ts ./
COPY --from=builder /app/postcss.config.mjs ./
COPY --from=builder /app/components.json ./
COPY --from=builder /app/node_modules ./node_modules

# Expose Next.js default port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]

```

## Penjelasan SonarQube

### Pendahuluan
SonarQube merupakan platform untuk menganalisis kualitas kode dan mendeteksi potensi celah keamanan, bugs, code smell, dan vulnerabilities pada proyek aplikasi. Pada tahap ini, SonarQube digunakan untuk melakukan evaluasi kode backend secara otomatis, sehingga potensi masalah pada codebase dapat diidentifikasi lebih awal.

### Langkah - Langkah
Menjalankan SonarQube
SonarQube telah berhasil diinstal pada VM Google Cloud dan dapat diakses melalui browser pada alamat:
http://<EXTERNAL_IP_VM>:9000

Login menggunakan akun administrator.

Proyek backend (nantinya) akan di-scan menggunakan scanner SonarQube sesuai bahasa pemrograman (misal: SonarScanner, Maven, Gradle, dsb).

b. Proses Analisis Kode
Source code backend di-scan menggunakan SonarQube Scanner dari local/CI pipeline.

Hasil analisis akan langsung muncul di dashboard SonarQube.

3. Contoh Hasil & Penjelasan Singkat
(Kamu bisa ganti screenshot/isi dengan hasil asli nanti)

a. Screenshot Dashboard SonarQube
[Masukkan gambar hasil scan di sini!]

b. Penjelasan Temuan Utama
Contoh penjelasan yang bisa kamu pakai/cicil (nanti ganti sesuai hasil nyata):

Rangkuman Hasil Scan:
Bugs: 0
Tidak ditemukan bug kritis pada source code.

Vulnerabilities: 1
Terdapat 1 potensi celah keamanan pada endpoint login, disarankan menambah validasi input user.

Code Smell: 5
Ditemukan 5 code smell, mayoritas terkait penggunaan function yang belum optimal (misal: duplikasi kode, penggunaan variable tidak efektif).

Security Hotspot: 1
Terdeteksi penggunaan library yang membutuhkan review terkait keamanan, namun belum terkonfirmasi sebagai vulnerability.

Coverage: 80%
Code coverage sudah mencapai 80% dari total kode, memenuhi standar minimal coverage.

Rekomendasi Tindak Lanjut:
Perbaiki potensi vulnerability yang ditemukan dengan melakukan validasi input yang lebih baik pada endpoint login.

Refactor code untuk mengurangi code smell, terutama pada bagian kode yang duplikat dan tidak optimal.

Review penggunaan library yang menjadi security hotspot.

4. Lampiran Screenshot
(Masukkan screenshot hasil scan: dashboard, issue list, dsb)

Dashboard utama SonarQube

Detail issues (bugs/vulnerabilities/code smell)

Grafik coverage/code quality



