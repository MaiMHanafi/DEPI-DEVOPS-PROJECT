# Use Node.js image
FROM node:18

# Set the working directory
WORKDIR /app

# Copy dependencies and install
COPY package*.json ./
RUN npm install

# Copy the backend source code
COPY . .

# Expose the backend port
EXPOSE 5000

# Start the backend server
CMD ["npm", "run", "dev-server"]

