# NextJS x TypeScript x Tailwind on CloudRun

Try it out with NextJS application

[![Run on Google Cloud](https://deploy.cloud.run/button.svg)](https://deploy.cloud.run)

Cloud Buildpacks を使えば Dockerfile いらないと思ったが、以下のような制限があるので無理そう

> - Node:
>   - Custom build steps (e.g. executing the "build" script of package.json) are not supported.
>   - Existing node_modules directory is deleted and dependencies reinstalled using package.json and a lockfile if present.
