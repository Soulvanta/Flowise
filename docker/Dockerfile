# Stage 1: Build stage
FROM node:20-alpine AS build

USER root

# Install build dependencies
RUN apk add --no-cache git python3 py3-pip make g++ build-base

# Set working directory
WORKDIR /app

# Copy local repo source code
COPY . .

# Install dependencies using npm
RUN npm install

# Build the Flowise app
RUN npm run build

# Stage 2: Runtime stage
FROM node:20-alpine

# Install runtime dependencies
RUN apk add --no-cache chromium git python3 py3-pip make g++ build-base cairo-dev pango-dev curl

ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Set app working directory
WORKDIR /app

# Copy built app from build stage
COPY --from=build /app /app

# Expose default Flowise port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
