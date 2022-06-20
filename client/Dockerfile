## Setup react
FROM node:12-alpine3.14 AS development
# Set working directory
WORKDIR /app
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
COPY package.json /app/package.json
COPY package-lock.json /app/package-lock.json
# Same as npm install
RUN npm ci
COPY . /app
CMD [ "npm", "run", "start" ]
FROM development AS build
RUN npm run build

## Set up nginx
FROM nginx:alpine
# Copy config nginx
COPY --from=build /app/.nginx/nginx.conf /etc/nginx/conf.d/default.conf
WORKDIR /usr/share/nginx/html
# Remove default nginx static assets
RUN rm -rf ./*
# Copy static assets from builder stage
COPY --from=build /app/build .
# Containers run nginx with global directives and daemon off
ENTRYPOINT ["nginx", "-g", "daemon off;"]