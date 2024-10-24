# Gunakan image Node.js versi 22 sebagai base image
FROM node:22

# Tentukan direktori kerja di dalam container
WORKDIR /app

# Salin file package.json dan pnpm-lock.yaml (jika ada) ke dalam container
COPY package.json pnpm-lock.yaml ./

# Install dependencies menggunakan pnpm (jika tidak ada pnpm-lock.yaml, ganti dengan npm install)
RUN npm install -g pnpm && pnpm install || npm install

# Salin seluruh kode ke dalam direktori kerja
COPY . .

# Ekspos port backend (misalnya 3000)
EXPOSE 3000

# Jalankan aplikasi Node.js menggunakan pnpm start
CMD ["pnpm", "start"]
