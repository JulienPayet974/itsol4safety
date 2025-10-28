# Build
FROM node:20-alpine AS build
WORKDIR /app
COPY next/package*.json ./
RUN npm ci
COPY next/ .
# Le starter codegen GraphQL pendant le build; npm run build s'en charge
RUN npm run build

# Runtime (standalone)
FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY --from=build /app/.next/standalone ./
COPY --from=build /app/public ./public
COPY --from=build /app/.next/static ./.next/static
EXPOSE 3000
CMD ["node", "server.js"]
