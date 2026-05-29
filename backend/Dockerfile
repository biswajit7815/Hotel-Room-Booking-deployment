FROM node:21 AS backend-builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

FROM node:21-alpine
WORKDIR /app
ENV NODE_ENV=production

RUN addgroup --system appgroup \
    && adduser -S appuser -G appgroup

# ✅ Copy FIRST, then switch user
COPY --from=backend-builder --chown=appuser:appgroup /app/package*.json ./
COPY --from=backend-builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=backend-builder --chown=appuser:appgroup /app .

USER appuser

EXPOSE 5000

HEALTHCHECK --interval=30s --timeout=10s --start-period=30s --retries=3 \
  CMD wget -q --spider http://localhost:5000/health || exit 1

CMD ["node", "server.js"]
