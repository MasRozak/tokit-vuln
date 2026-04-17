FROM node:20-alpine AS builder

WORKDIR /app

# Copy dependency files
COPY package*.json ./
RUN npm install

# Copy kode aplikasi
COPY . .

# Build Next.js application
# Note: Next.js reads env vars during build. This default can be overridden via docker-compose
ENV NEXT_PUBLIC_API_URL=http://localhost:5000
RUN npm run build

# Production image
FROM node:20-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Copy package.json dan node_modules production
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public

EXPOSE 3000

CMD ["npm", "start"]
