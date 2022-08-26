<<<<<<< HEAD
FROM node:16-alpine3.12 as builderWORKDIR /app
=======
FROM node:16-alpine3.12 as builder
WORKDIR /app
>>>>>>> main
COPY ./package.json .
RUN npm install
COPY . .
EXPOSE 3000
RUN npm run build
FROM nginx
EXPOSE 3000
COPY --from=builder /app/build /usr/share/nginx/html
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
CMD ["nginx", "-g", "daemon off;"]
