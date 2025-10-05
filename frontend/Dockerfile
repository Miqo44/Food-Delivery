# 1) build stage
FROM node:20-alpine AS builder
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH

# install deps
COPY package*.json ./
RUN npm ci --silent

# copy source and build
COPY . .
RUN npm run build

# 2) production stage - nginx serves the static build
FROM nginx:stable-alpine

# Vite создаёт dist, а не build
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
