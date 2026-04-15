# Stage 1: Build the Vite application
FROM node:20-alpine AS build

WORKDIR /app

# Install dependencies based on package-lock
COPY package*.json ./
RUN npm install

# Build arguments for environment variables
ARG VITE_API_BASE_URL
ARG VITE_USE_BACKEND
ARG VITE_WS_URL
ARG VITE_TODO_WS_URL

ENV VITE_API_BASE_URL=$VITE_API_BASE_URL
ENV VITE_USE_BACKEND=$VITE_USE_BACKEND
ENV VITE_WS_URL=$VITE_WS_URL
ENV VITE_TODO_WS_URL=$VITE_TODO_WS_URL

# Copy source and build
COPY . .
RUN npm run build

# Stage 2: Serve the application with Nginx
FROM nginx:stable-alpine

# Copy custom Nginx config for SPA
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built assets
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
