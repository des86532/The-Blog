---
title: 用 Github 的 gh-pages 分支展示自己的項目(Vue-cli 3)
comments: false
date: 2019-06-15
categories:
    - Git
tags:
    - git
    - github
    - github pages
---

## 步驟：

1. 建立一個檔案 `vue.config.js` ，此為 vue-cli 3 的配置設定檔
```
// vue.config.js
module.exports = {
 baseUrl: ‘/my-first-project/’ 
// baseUrl 為 project-name，根目錄地址為上傳的網域
}
```

2. 執行 `npm run build` 生成 **dist**

3. 將 **dist** 從 **.gitignore** 移除

> [Vue-cli 3](https://cli.vuejs.org/config/#baseurl)

4. 執行 `git add dist` && `git commit -m “Initial dist subtree commit”`

這段是將 **dist** 也加入 git 的儲存庫，並且 **commit**

5. `git subtree push –prefix dist origin gh-pages`

將 **dist** push 到 **gh-pages**

## 另一種方法：
```
git run build
git branch gh-pages     //创建gh-pages分支
git checkout gh-pages   //切换到gh-pages分支
git add -f dist         //强制把dist文件夹提交到github
$ git subtree push --prefix dist origin gh-pages    //把dist文件夹单独部署到 gh-pages 分支
```

在真正的項目開發中，**dist** 文件夾最終是要放到服務器上，而不是 github 上，所以 **.gitignore** 內才會有 **dist**

> [Deploy vue-cli 3 project to github pages](https://medium.com/@Roli_Dori/deploy-vue-cli-3-project-to-github-pages-ebeda0705fbd)
[vue项目实现github在线预览效果](https://juejin.im/post/5b276644e51d4558a3051b5e)