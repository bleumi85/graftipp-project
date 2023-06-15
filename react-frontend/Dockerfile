# Use the official Node.js 18 image as the base image
FROM node:18-alpine as builder

# Set the working directory inside the container
WORKDIR /app

# Copy the package.json and package-lock.json files for the React app
COPY package.json package-lock.json ./

# Install dependencies for the React app
RUN npm ci --production

# Copy the rest of the React app code
COPY . .

# Build the React App
RUN npm run build

# Use the official Nginx image as the base image for serving the React app
FROM nginx:alpine as nginx

# Remove the default Nginx configuration and copy our own configuration file
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the built React app from the previous stage to the Nginx default public directory
COPY --from=builder /app/build /usr/share/nginx/html

# Expose port 80 for the Nginx server
EXPOSE 80

# Start the Nginx server when the container is run
CMD ["nginx", "-g", "daemon off;"]