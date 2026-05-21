# ============================================================
# Dockerfile - Frontend (React / Create React App)
# Đặt file này tại: frontend/Dockerfile
# ============================================================

# --- Stage 1: Build React app ---
FROM node:16-alpine AS builder

WORKDIR /app

# Copy package files trước để cache layer khi code thay đổi
COPY package*.json ./

# CRA cũ cần --legacy-peer-deps để tránh conflict
RUN npm install --legacy-peer-deps

# Copy toàn bộ source code frontend
COPY . .

# Build production bundle → output ra /app/build
RUN npm run build

# --- Stage 2: Serve với Nginx ---
FROM nginx:1.25-alpine

# Xóa config mặc định của nginx
RUN rm /etc/nginx/conf.d/default.conf

# Copy nginx config tuỳ chỉnh (xử lý React Router + proxy API)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy static files đã build
COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget -qO- http://localhost:80 || exit 1

CMD ["nginx", "-g", "daemon off;"]
