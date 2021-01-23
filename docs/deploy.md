# CloudRun へのデプロイ

[Create a Front App immediately with Next.js on Google Cloud Run | by Toru Eguchi | Jan, 2021 | ITNEXT](https://itnext.io/create-a-front-app-immediately-with-next-js-on-google-cloud-run-d0cfde795ce3)

- `Dockerfile`作成
- `.dockerignore`作成
- `PORT` 環境変数の設定

```shell
# base image
FROM node:15.2.1-alpine
# working directory
WORKDIR /app
# add binaries to $PATH
ENV PATH /app/node_modules/.bin:$PATH
# install and cache app dependencies
COPY package.json /app/
COPY yarn.lock /app/
RUN yarn install
# copy app files and build
COPY . /app
RUN yarn build
# start app
CMD [ "yarn", "start" ]
```

Next.js のデフォルトのポート設定は 3000 であるため、-p オプションを指定してポートを変更します。
cloudrun 注入する PORT 変数で解放されるようにしてい
`"start": "next start -p $PORT"`

`docker build . -t next-app:latest`
`docker run --rm --name next -e PORT=8080 -p 4000:8080 next-app`

-e オプションを介して環境変数を渡します。ブラウザリクエストは 4000 を受け入れ、8080 に転送され、8080 で Next.js アプリを起動します。

# cloudrun にデプロイする

## CloudBuildpacks

[たった一つで Cloud Run にビルドしてデプロイするコマンドの紹介 | Google Cloud Blog](https://cloud.google.com/blog/ja/products/serverless/build-and-deploy-an-app-to-cloud-run-with-a-single-command)

`gcloud beta run deploy my-app --source .`

## BuildPacks 使わない方法

[Docker のクイックスタート  |  Artifact Registry のドキュメント  |  Google Cloud](https://cloud.google.com/artifact-registry/docs/docker/quickstart)
[Cloud Run へのデプロイ  |  Artifact Registry のドキュメント  |  Google Cloud](https://cloud.google.com/artifact-registry/docs/integrate-cloud-run#command-line)

1. 別の名前空間 Docker イメージタグを作成する
1. Docker イメージを GoogleArtifact Registry（GAR）にプッシュします
1. プッシュされたイメージを CloudRun にデプロイします

以下の API を有効化しておく

> todo GCP のドキュメント

```shell
docker push gcr.io/<GCP_PROJECT_NAME>/<GCR_REPO_NAME>:<GCR_TAG>
```

```bash
$ gcloud beta run deploy --image gcr.io/test-project/next-app:latest \
        --project test-project \
        --platform managed \
        --region asia-northeast1 \
        --allow-unauthenticated
```

# CD

> todo
