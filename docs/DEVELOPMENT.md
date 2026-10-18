# 本地开发

在线体验无需运行源码：[打开麦有谱](https://suikv000.github.io/mai-you-pu/)。

以下步骤仅供下载源码后本地开发使用。需要 Python 3，无第三方依赖。

```sh
git clone https://github.com/suikv000/mai-you-pu.git
cd mai-you-pu
python3 -m http.server 8768 --bind 127.0.0.1 --directory dist
```

服务启动后，在同一台电脑打开 `http://127.0.0.1:8768/`。这个地址只指向自己的电脑，不是公开演示网址。

配方修改需同步 `dist/recipes.json` 和 `skills/mai-you-pu/references/recipes.json`。GitHub Pages 从 `gh-pages` 分支根目录发布，对应主分支的 `dist` 内容。
