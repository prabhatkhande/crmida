# Stage 1: Build Stage
FROM node:23 AS build

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./

# Install Node.js dependencies (including dev dependencies if any)
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Stage 2: Production Stage
FROM node:23-slim AS production

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy only the necessary files from the build stage
COPY --from=build /usr/src/app /usr/src/app

# Install only production dependencies (without dev dependencies)
RUN npm install --production

# Expose the backend port (5000)
EXPOSE 5000

# Start the backend application
CMD ["npm", "start"]