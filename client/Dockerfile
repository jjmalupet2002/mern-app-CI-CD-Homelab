# --- Base image ---
FROM node:16.20.2-buster-slim as base
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install

# --- Development stage ---
FROM base as dev
WORKDIR /usr/src/app
COPY . .
RUN chown -R node:node /usr/src/app
EXPOSE 8080
CMD ["npm", "run", "dev"]   # or "start" if you're using CRA

# --- Production build ---
FROM base as build
WORKDIR /usr/src/app
COPY . .
RUN mv .env.example .env
RUN npm run build

FROM nginx:alpine as prod
COPY --from=build /usr/src/app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]
