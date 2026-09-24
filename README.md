# chin-all.github.io

各个项目需要公网访问的页面。纯静态 HTML，没有构建步骤。

```
index.html               → https://chin-all.github.io/                 全站索引
404.html                 → 找不到页面时自动显示
.nojekyll                → 空文件，见下
momo/privacy/index.html  → https://chin-all.github.io/momo/privacy/    App Store「隐私政策 URL」
momo/support/index.html  → https://chin-all.github.io/momo/support/    App Store「支持 URL」
```

## 加一个新页面

1. 建目录放 `index.html`，例如 `某项目/某页面/index.html`
2. 在根目录 `index.html` 里照着已有条目复制一条（新项目复制整组 `<section>`）
3. commit + push，一两分钟后生效

用「目录/index.html」而不是「xxx.html」：地址是 `/momo/privacy/`，
填进 App Store Connect 之后，哪天换实现方式也不用改链接。

## `.nojekyll` 为什么要有

GitHub Pages 默认会用 Jekyll 处理仓库，而 Jekyll **会忽略下划线开头的文件和目录**。
现在用不上，但哪天放进一个 `_assets/` 之类的目录，它会静默 404，而且很难想到是这个原因。
一个空文件换一个永远不会踩的坑。

## ⚠️ 已经填进 App Store Connect 的地址不要改

`momo/privacy/` 和 `momo/support/` 这两个路径一旦上架就绑定了。
挪位置 = 审核员和用户点开是 404，而 Apple 会因为隐私政策打不开而下架或拒审。
要重组目录的话，旧路径留一个跳转页。

---

## 首次初始化（只做一次）

旧仓库是 Hexo 博客的生成产物。下面这套做法是**清空历史、从零开始**。

```bash
# 1. 克隆旧仓库，建一个没有任何历史的新分支
git clone https://github.com/chin-all/chin-all.github.io.git
cd chin-all.github.io
git checkout --orphan fresh

# 2. 删掉旧博客的全部文件（只删工作区，.git 保留）
git rm -rf . > /dev/null
rm -rf ./*    # 兜底清掉未跟踪的残留

# 3. 把这个目录里的东西复制进来（注意 .nojekyll 是隐藏文件，要用 /. 结尾才会带上）
cp -R /Users/chinyan/Documents/myCode/ai_pets/publish/chin-all.github.io/. .

# 4. 提交，替换掉原来的主分支
git add -A
git commit -m "重新初始化：公开页面索引 + momo 隐私政策与支持页"
git branch -D main          # 旧仓库的默认分支就是 main（2026-09-24 查过）
git branch -m main
git push -f origin main
```

**第 4 步是强推，旧博客的全部历史会从 GitHub 上消失。** 如果想留一份，
推之前先在 GitHub 上把旧仓库 fork 一份，或者本地 `git branch old-blog origin/main` 留着。

推完之后去仓库 **Settings → Pages** 确认：
Source 是 **Deploy from a branch**，Branch 是 **main / (root)**。
旧仓库本来就是这么配的，一般不用动；看一眼只是为了排除「它还指着别的分支」这种情况。

### 检查

浏览器打开这三个地址，都能看到新页面：

- https://chin-all.github.io/
- https://chin-all.github.io/momo/privacy/
- https://chin-all.github.io/momo/support/

看到的还是旧博客的话，多半是浏览器缓存或 Pages 还没部署完——
仓库 **Actions** 页能看到 `pages build and deployment` 的进度。

### 旧博客留下的两样东西

- **百度 / Google 的站点验证**：旧首页里有 `google-site-verification` 和
  `baidu-site-verification` 两个 meta 标签，新首页没带。
  不再用这两个站长平台就不用管；还要用的话，把那两行加回 `index.html` 的 `<head>`
- **旧文章链接**：`/posts/xxx/` 这类地址以后都会落到 `404.html`，那一页有回首页的链接
