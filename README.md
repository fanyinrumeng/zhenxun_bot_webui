# web-ui

## 前后端分离配置修改

* 在真寻目录下，修改`~/.env.dev`文件，将 `HOST = 127.0.0.1` 修改为 `HOST = 0.0.0.0` ，并在安全组和防火墙打开相应的端口
* 修改插件 `~/plugins/web_ui/config.py` 文件，修改`origins = ["http://localhost"]`为`origins = ["*"]`，亦或者`origins = ["你的真寻web自定义域名"]`

## 在 Vercel 中部署

* `fork`本项目，然后进入自己的项目中

* 进入 `vercel.json` 文件内，将 `${zhenxun_URL}` 改成你的真寻的公网ip加端口。eg: `http://127.0.0.1:8080`

* 之后直接在 Vercel 中选择部署即可

## 以下是阿咪的部署流程(雾

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
