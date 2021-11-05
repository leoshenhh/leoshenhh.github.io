作为一个强迫症患者，总是想让博客的源代码和部署在一个仓库（两个仓库总感觉很难受~）,更新代码就能自动构建部署。如果你也一样的话，可以参考下面的流程

> ps： 我在网上搜到的很多教程要搞私钥公钥啥的根本搞不定~~(我太菜了)

`master`分支为博客源代码，`gh-pages`分支为博客部署分支

+ 以下所有`<username>` 替换为你的 **github用户名**

##### 全局安装 `hexo-cli` 构建工具

```js
npm install -g hexo-cli
```

##### 初始化hexo仓库

```js
npx hexo init <username>.github.io
```

##### 进入hexo仓库，本地运行这个博客,如果本地能访问到 说明你成功啦

```js
hexo server
```

##### github上创建一个仓库,仓库名设置为

```js
<username>.github.io
```

##### 进入这个仓库，把这个仓库推到github上

```js
git init
git git remote add origin https://github.com/<username>/<username>.github.io
git add .
git commit -m init
git push -u origin master
```

##### 在`.github/workflows` 文件夹中新建`pages.yml` 文件、

+ `pages`就是总标题

+ `name` 就是每一步的标题

+ `${{}}`表示从环境变量中读取

  [![IuOfOI.png](https://z3.ax1x.com/2021/11/05/IuOfOI.png)](https://imgtu.com/i/IuOfOI)

```js
name: Pages

on:
  push:
    branches:
      - master  # 监听master分支的push 如果触发就执行action

jobs:
  pages: # 总标题
    runs-on: ubuntu-latest # 启动的虚拟机的系统
    steps: # action步骤
      - uses: actions/checkout@v2 # 拉取当前master分支代码
      
      - name: Use Node.js 12.x # 下面uses这一步的标题
        uses: actions/setup-node@v1 # 执行的命令 设置安装node
        with:
          node-version: '12.x' # 设置node版本为12.x
          
      - name: Cache NPM dependencies
        uses: actions/cache@v2 # 执行的命令 读取npm缓存
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache

      - name: Install Dependencies
        run: npm install # 执行的命令 npm install 相信各位经常使用
        
      - name: Build
        run: npm run build # 不用说了吧
        
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3 # 用的是第三方的actions
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }} # 从环境变量中获取的githubtoken
          publish_dir: ./public # 从哪个文件夹中取文件
          publish_branch: gh-pages  # 要部署到哪个个分支 branch
```

##### 编辑`_config.yml`  文件

```js
deploy:
  type: git
  repo: https://github.com/<username>/<username>.github.io
  branch: gh-pages
```

##### 在仓库的`Settings` 选项中将默认构建分支改为`gh-pages`

[![IuXBcj.md.png](https://z3.ax1x.com/2021/11/05/IuXBcj.md.png)](https://imgtu.com/i/IuXBcj)

+ 现在访问`<username>.github.io`就应该看到你的博客啦，如果404的话，就在博客随便改个内容再次触发构建



完美，强迫症患者舒服了。如果你也舒服了，请点个赞吧~
