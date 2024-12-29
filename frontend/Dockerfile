# Stage 1: Build the frontend assets using Node.js
FROM node:23 AS builder

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy only package.json and package-lock.json (or npm-shrinkwrap.json) to leverage Docker cache
COPY package*.json ./

# Install the dependencies
RUN npm install

# Copy the rest of the application code (source files) into the container
COPY . .

# Run the build script to generate the dist folder (production assets)
RUN npm run build

# Stage 2: Serve the built static files using NGINX
FROM nginx:alpine AS production

# Copy the custom nginx configuration to the appropriate directory
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the build output (dist folder) from the builder stage into the NGINX server directory
COPY --from=builder /usr/src/app/dist /usr/share/nginx/html

# Expose port 80 for the HTTP service
EXPOSE 80

# Run NGINX in the foreground to keep the container alive
CMD ["nginx", "-g", "daemon off;"]
