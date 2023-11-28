# Use the official Node.js image as the base image
FROM node:19-alpine

ARG NODE_ENV DATABASE_URL DATABASE_HOST DATABASE_PORT

# Needed for the wait-for-db script
RUN apk add --no-cache netcat-openbsd

# Set the working directory
WORKDIR /next

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install -g husky
RUN npm ci

# Copy the wait-for-db.sh script
COPY wait-for-db.sh /usr/local/bin/wait-for-db.sh
RUN chmod +x /usr/local/bin/wait-for-db.sh

# Copy the rest of the application code
COPY . .
COPY entrypoint.sh /

# Ensure correct line endings after these files are edited by windows
RUN apk add --no-cache netcat-openbsd
RUN npm run build

# Expose the port the app will run on
EXPOSE 3000

ENTRYPOINT ["sh", "/entrypoint.sh"]

# Start the application
CMD ["npm", "run", "start"]
