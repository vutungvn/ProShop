# ============================================================
# Dockerfile - Backend (Node.js / Express)
# Đặt file này tại: backend/Dockerfile
# ============================================================

FROM node:16-alpine

WORKDIR /app

# Copy package files trước để cache layer
COPY package*.json ./ 

# Dùng npm install 
RUN npm install --omit=dev

# Copy toàn bộ source code backend
COPY . .

# Tạo thư mục uploads (nếu backend lưu file upload tại đây)
RUN mkdir -p /app/uploads

# Expose cổng backend (mặc định 5000)
EXPOSE 5000

# Healthcheck: kiểm tra backend còn sống không
HEALTHCHECK --interval=30s --timeout=10s --start-period=20s --retries=3 \
    CMD wget -qO- http://localhost:5000/api/products?pageNumber=1 || exit 1

# Chạy server bằng node (không dùng nodemon trong production)
CMD ["node", "backend/server.js"]