FROM node:16-alpine as builder-stage
WORKDIR /app

COPY package*.json ./

RUN apk --no-cache upgrade && \
    apk add --no-cache busybox openssl-dev 

RUN npm install
COPY . .
RUN npm run build

FROM nginx:stable-alpine as production-stage
COPY --from=builder-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]