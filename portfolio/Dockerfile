### ベースステージ ###
FROM node:18.17.0


# @angular/cliをグローバルインストール
RUN npm install -g @angular/cli@16.2.0

# ワーキングディレクトリの設定
WORKDIR /portfolio

# package.jsonをコピー
COPY ./package*.json /portfolio

# 一度node_modulesを削除してからnpm install
RUN rm -rf node_modules && npm install


### ビルドステージ ###
FROM base as build

# 全てのソースファイルをコピー
COPY ./ /portfolio/

# 本番用ビルド
RUN ng build --prod --output-path=./dist/build-by-docker


### プロダクションステージ ###
FROM nginx:alpine as prod

# ビルドステージで生成されたファイルをnginxの公開用ディレクトリにコピー
COPY --from=build /portfolio/dist/build-by-docker /usr/share/nginx/html

# nginx.confをコピー
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

