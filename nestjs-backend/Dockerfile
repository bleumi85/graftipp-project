FROM node:18-alpine as base

WORKDIR /app
COPY package.json package-lock.json ./
EXPOSE 3000 9229

FROM base as development
RUN npm i -g rimraf
RUN npm ci --only=development
COPY . .
RUN npm run build

FROM base as production
ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}
RUN npm ci --only=production
COPY . .
COPY --from=development /app/dist ./dist

CMD ["node", "dist/main"]