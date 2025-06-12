# Build stage
FROM node:20-alpine as build

WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json* ./
RUN npm ci

# Copy all frontend files and build
COPY . .
RUN npm run build

# Production stage
FROM docker.io/nginx:alpine

# Copy built files from build stage to nginx serve directory
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]