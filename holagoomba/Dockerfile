FROM node:18-alpine AS builder

WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json .

# Install dependencies
RUN npm ci 

# Copy the rest of the files, after installing dependencies to take advantage of docker's layer caching mechanism
COPY . .

# Build the app
RUN npm run build

# Remove dev dependencies, as we no longer need them
RUN npm prune --production

# Last stage
FROM node:18-alpine

WORKDIR /app

COPY --from=builder /app/build build/
COPY --from=builder /app/node_modules node_modules/
COPY package.json .

EXPOSE 3000

ENV NODE_ENV=production

CMD [ "node", "build" ]