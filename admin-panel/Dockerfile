# ------------------------------------------------------------------------------
# 🏗️ Stage 1: Build React Admin Panel
# ------------------------------------------------------------------------------

FROM node:20-alpine AS builder

WORKDIR /app

# copy package files
COPY package*.json ./

# install dependencies
RUN npm ci

# copy source code
COPY . .

# build argument
ARG REACT_APP_API_BASE_URL

# production environment
ENV NODE_ENV=production
ENV REACT_APP_API_BASE_URL=${REACT_APP_API_BASE_URL}

# build react app
RUN npm run build


# ------------------------------------------------------------------------------
# 🚀 Stage 2: Serve React App Using Nginx
# ------------------------------------------------------------------------------

FROM nginx:1.25-alpine

# copy nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# copy production build files
COPY --from=builder /app/build /usr/share/nginx/html

# expose nginx port
EXPOSE 80

# start nginx
CMD ["nginx", "-g", "daemon off;"]

