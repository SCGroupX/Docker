FROM registry.cn-shenzhen.aliyuncs.com/nodedev/nodejs:10.12.0-alpine

ADD . /app/
WORKDIR /app

RUN npm install
RUN npm rebuild node-sass --force

ENV HOST 0.0.0.0

ENV PORT 8000

EXPOSE 8000
CMD ["npm","run","dev"]