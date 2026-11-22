# 猪周期六信号观测台 · 自托管部署包

这个文件夹里已经放好了一份可直接上线的网站，没有任何平台水印。

## 目录结构

```
deploy_github/
├── index.html                     ← 网站本体（单文件，含全部样式与逻辑）
└── .github/workflows/pages.yml    ← GitHub Pages 自动发布流水线
```

## 第一次上线：三步

1. **建仓库**
   在 GitHub 新建一个仓库，名字随意（例如 `pig-cycle-dashboard`），
   可见性选 **Public**（Pages 免费版要求公开仓库）。

2. **把本文件夹的内容推上去**
   在这个 `deploy_github` 文件夹里打开 PowerShell，把下面三行的 `<你的用户名>` 和 `<仓库名>` 换成自己的：

   ```powershell
   git init
   git add .
   git commit -m "init"
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git branch -M main
   git push -u origin main
   ```

3. **打开 Pages**
   进仓库 → Settings → Pages → Source 选 **GitHub Actions**。
   等一两分钟，页面地址就是：
   `https://<你的用户名>.github.io/<仓库名>/`

   之后每次 `git push`，几十秒后线上自动更新。

## 日常更新

- 我改完 `index.html`，你（或我）执行：

  ```powershell
  git add . ; git commit -m "weekly update" ; git push
  ```

- 每周二的定时任务只需要把最后一步从「调用豆包发布接口」改成上面的推送命令，
  其他抓数、写数组、校验的步骤完全不变。

## 换成 Cloudflare Pages（可选）

Cloudflare 国内访问更稳，步骤：登录 Cloudflare → Workers & Pages → Create →
Pages → Connect to Git → 选这个仓库 → Build command 留空，Output directory 填 `/`。
不需要 workflow 文件，Cloudflare 自己会在推送后构建。

## 注意

- 页面数据写在 `index.html` 的 `const SEED = [...]` 数组里，改动时不要碰其他逻辑。
- 历史周数据只存在浏览器本地（localStorage），换电脑不带走，需要留档用页面里的「导出 CSV」。
- 这个网站不联网抓数，靠每周录入或定时任务写入，属于静态站点，不需要后端。
