# Use an ARM64 compatible Node image for the build stage
FROM node:16-buster-slim AS ionic
# FROM arm64v8/node:16-buster-slim AS ionic

# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install Ionic
RUN npm install -g @ionic/cli

# Create app directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install app dependencies
RUN npm install --force

# Bundle app source
COPY . .

# Build the app
RUN npx ionic build

# Use an ARM64 compatible Nginx image for the run stage
FROM nginx:alpine
# FROM arm64v8/nginx:alpine

# Copy built files from the Ionic build stage
COPY --from=ionic /usr/src/app/www /usr/share/nginx/html


CMD ["nginx", "-g", "daemon off;"]