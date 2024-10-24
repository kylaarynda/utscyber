# Gunakan Node.js versi 22 sebagai base image untuk build aplikasi Vue.js
FROM node:22 as build

# Tentukan direktori kerja di dalam container
WORKDIR /app

# Salin file package.json dan pnpm-lock.yaml ke direktori kerja
COPY package.json pnpm-lock.yaml ./

# Install dependencies menggunakan pnpm (fallback ke npm jika pnpm gagal)
RUN npm install -g pnpm && pnpm install || npm install

# Salin seluruh kode ke dalam direktori kerja
COPY . .

# Build aplikasi untuk produksi
RUN pnpm run build || npm run build

# Gunakan image nginx untuk serve file statis dari hasil build
FROM nginx:alpine

# Salin file build dari tahap build ke nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Ekspos port untuk nginx (biasanya port 80)
EXPOSE 80

# Jalankan nginx
CMD ["nginx", "-g", "daemon off;"]
