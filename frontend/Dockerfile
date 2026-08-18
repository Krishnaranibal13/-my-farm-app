# Stage 1: Build
FROM node:20-alpine AS builder

WORKDIR /app

# Install dependencies first for better Docker layer caching
COPY package*.json ./

RUN npm ci

# Copy application source
COPY . .

# Build Next.js application
RUN npm run build


# Stage 2: Production
FROM node:20-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Next.js standalone output
COPY --from=builder /app/.next/standalone ./

# Static files
COPY --from=builder /app/.next/static ./.next/static

# Public files
COPY --from=builder /app/public ./public

EXPOSE 3000

CMD ["node", "server.js"]
