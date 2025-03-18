# Build Stage
FROM node:22.12-alpine AS builder

WORKDIR /app
COPY . .

# Install dependencies & build the project
RUN --mount=type=cache,target=/root/.npm npm install && npm run build

# Production Stage
FROM node:22-alpine AS release

WORKDIR /app

# Copy necessary files from the build stage
COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json
COPY --from=builder /app/node_modules /app/node_modules

# Set environment
ENV NODE_ENV=production

# 🔥 This is important for Railway to detect the correct port
EXPOSE 3001 

# Start the MCP server
ENTRYPOINT ["node", "dist/index.js", "--server", "--host", "0.0.0.0", "--port", "3001"]
