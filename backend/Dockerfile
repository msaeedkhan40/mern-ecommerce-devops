FROM node:22-bookworm-slim

WORKDIR /app

ENV NODE_ENV=production

COPY package*.json ./

RUN npm ci --omit=dev

COPY . .

RUN mkdir -p uploads

EXPOSE 5000

CMD ["npm", "start"]
