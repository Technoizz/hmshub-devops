FROM node:alpine as build-stage

ARG NPN_ENV=development

WORKDIR /app

COPY package*.json ./

RUN npm install --silent

RUN npm install react-scripts@4.0.1 -g --silent

COPY src ./src

RUN npm run build:${NPN_ENV}

#Pipeline do nginx

FROM nginx:alpine as production-stage

RUN rm /etc/nginx/conf.d/default.conf

COPY --from=build-stage /src/app/build /usr/share/nginx/html

COPY ../reverse_proxy/nginx/nginx.conf /etc/nginx/conf.d

EXPOSE 80

CMD ["nginx","-g","daemon off;"]