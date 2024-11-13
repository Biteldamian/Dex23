FROM node:18-alpine

WORKDIR /app

# Set environment variables
ENV NEXT_TELEMETRY_DISABLED=1
ENV NODE_ENV=production

# Copy package files
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci

# Copy only frontend files
COPY app/ ./app/
COPY public/ ./public/
COPY components/ ./components/
COPY lib/ ./lib/
COPY styles/ ./styles/
COPY next.config.js .
COPY postcss.config.js .
COPY tailwind.config.ts .
COPY tsconfig.json .
COPY .env.local .

# Build application
RUN npm run build

# Expose port
EXPOSE 3000

# Start application
CMD ["npm", "start"]