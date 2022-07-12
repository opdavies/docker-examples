FROM node:16 AS base

ENV PATH=${PATH}:/home/node/.yarn/bin

WORKDIR /app

RUN mkdir /node_modules \
  && chown node:node -R /app /node_modules

USER node

RUN yarn global add vite

COPY --chown=node:node package.* *yarn* ./

RUN yarn --frozen-lockfile

COPY --chown=node:node . .

CMD ["bash"]
