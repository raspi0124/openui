# ORIGINAL FROM: https://note.com/minato_kame/n/n04cbd62e4e6f
# ==================================================
# パッケージのインストールのレイヤー
# ==================================================
FROM node:22-alpine AS deps
WORKDIR /base

COPY package.json package-lock.json ./
RUN  npm install --production


# ==================================================
# アプリのビルドレイヤー
# ==================================================
FROM node:22-alpine AS builder
WORKDIR /build
COPY --from=deps /base/node_modules ./node_modules

COPY ./app /build/app
COPY package.json yarn.lock ./

# buildは、tsからjsに変換するのでこれがないとエラーになる
COPY tsconfig.json .

RUN npm run build


# ==================================================
# アプリの実行レイヤー
# ==================================================
FROM node:22-alpine AS runner
#
WORKDIR /app

COPY --from=builder /build/node_modules ./node_modules
COPY --from=builder /build/package.json ./package.json

CMD ["npm", "start"]