# ------------------------------------------------------------------------------
# 🏗️ Stage 1: Install Dependencies
# ------------------------------------------------------------------------------

FROM node:21-alpine AS deps

WORKDIR /app

# copy package files
COPY package*.json ./

# install dependencies
RUN npm ci


# ------------------------------------------------------------------------------
# 🏗️ Stage 2: Build Next.js Application
# ------------------------------------------------------------------------------

FROM node:21-alpine AS builder

WORKDIR /app

# copy installed node_modules
COPY --from=deps /app/node_modules ./node_modules

# copy source code
COPY . .

# production environment
ENV NODE_ENV=production

# build nextjs app
RUN npm run build


# ------------------------------------------------------------------------------
# 🚀 Stage 3: Production Runner
# ------------------------------------------------------------------------------

FROM node:21-alpine AS runner

WORKDIR /app

# production environment
ENV NODE_ENV=production

# create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# copy required production files only
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules

# change ownership
RUN chown -R appuser:appgroup /app

# switch to non-root user
USER appuser

# expose nextjs port
EXPOSE 3000

# start nextjs server
CMD ["npm", "start"]

