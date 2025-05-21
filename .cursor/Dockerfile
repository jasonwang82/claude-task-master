# Build stage
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build the application (if needed)
RUN npm run build

# Production stage
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install only production dependencies
RUN npm ci --only=production

# Copy built files from builder stage
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/bin ./bin
COPY --from=builder /app/scripts ./scripts
COPY --from=builder /app/src ./src
COPY --from=builder /app/tasks ./tasks
COPY --from=builder /app/index.js .
COPY --from=builder /app/.taskmasterconfig ./.taskmasterconfig

# Set environment variables
ENV NODE_ENV=production

# Expose port if needed (adjust as necessary)
EXPOSE 3000

# Start the application
CMD ["node", "index.js"]
