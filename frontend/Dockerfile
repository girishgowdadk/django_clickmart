# Stage 1: Build
#node version 18 is the latest LTS version as of 2023-09-15
FROM node:18 AS build

# Set working directory
WORKDIR /app

# Install dependencies as we do in python, we need to install dependencies for our React app. We copy the package.json and package-lock.json files first to leverage Docker's caching mechanism. This way, if we don't change our dependencies, Docker will use the cached layer and won't reinstall them every time we build the image.
COPY package*.json ./
RUN npm install

# Copy the rest of the application code to the working directory. This includes all the source code, configuration files, and any other necessary files for the React app to run.
COPY . .

# Build arguments for environment variables
ARG VITE_SERVER_BASE_URL

# This line passes an environment variable into the Docker container so the React app knows the backend API URL.
ENV VITE_SERVER_BASE_URL=$VITE_SERVER_BASE_URL

RUN npm run build

# Stage 2: Nginx, alpine means the lighter version of Nginx
FROM nginx:alpine

# Copy build output to Nginx html directory
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]