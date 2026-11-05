FROM node:22-alpine AS builder

RUN apk update && apk upgrade --no-cache

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci

COPY public ./public
COPY src ./src

ENV NODE_ENV=production

RUN npm run build


FROM nginx:1.30-alpine

RUN apk update && apk upgrade --no-cache

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/conf.d/travelo.conf

COPY --from=builder /app/build /usr/share/nginx/html

RUN chown -R nginx:nginx /usr/share/nginx/html

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]