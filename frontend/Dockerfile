# Use Node.js for building the React app
FROM node:18 as builder

# Set the working directory
WORKDIR /app

# Copy dependencies and install
COPY package*.json ./
RUN npm install

# Copy source code and build the app
COPY . .
RUN npm run build

# Use Nginx to serve the React app
FROM nginx:alpine

# Copy built frontend files to Nginx
COPY --from=builder /app/build /usr/share/nginx/html

# Expose frontend port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
