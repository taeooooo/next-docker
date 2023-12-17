FROM node:18-alpine AS base
FROM nginx:1.25.3-alpine AS nginx

FROM base AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

COPY package.json package-lock.json*  ./
RUN npm ci


FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build -w packages/b 


FROM nginx AS runner
COPY --from=builder /app/packages/b/out /usr/share/nginx/html