FROM node:20-alpine

WORKDIR /app

# Copy package info dan install dependencies
COPY package*.json ./
RUN npm install

# Copy source code backend
COPY . .

# Expose port
EXPOSE 5000

# Set environment production
ENV NODE_ENV=production

# Jalankan server
CMD ["npm", "start"]
