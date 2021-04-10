FROM node:14.16.0-alpine3.13

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

RUN addgroup app && adduser -S -G app app
USER app

EXPOSE 3000 

CMD ["npm", "start"]