FROM node:20.20.2-alpine AS builder
WORKDIR /app

RUN apk update && apk upgrade && \
    apk add --no-cache build-base 

COPY package*.json ./

RUN npm ci --only=production

COPY . .

RUN adduser -D appuser && chown -R appuser:appuser /app

FROM node:20.20.2-alpine

ENV NODE_ENV=production
WORKDIR /app

RUN adduser -D appuser && mkdir -p /home/appuser/.npm && \
    chown -R appuser:appuser /home/appuser

COPY --from=builder /app /app
COPY --from=builder /etc/passwd /etc/passwd

RUN chown -R appuser:appuser /app
USER appuser
EXPOSE 3003
CMD ["npm","start"]

