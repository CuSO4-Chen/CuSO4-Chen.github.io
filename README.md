# 学术主页

纯静态 HTML + CSS，无构建步骤，推到 GitHub 即生效。
版式复刻 [cooperleong00.github.io](https://cooperleong00.github.io/) 所用的 minimal-light / minimal-mistakes 风格。

```
index.html      所有内容在这里改
style.css       版式与配色
images/         头像与论文缩略图
```

本地预览：双击 `index.html`；或 `python3 -m http.server 8000` 后访问 `localhost:8000`。

---

## 一、部署（5 分钟）

选**用户主页**形式：

1. 新建仓库，名字**必须**是 `<你的用户名>.github.io`，设为 Public。
2. 推送：

```bash
cd path/to/homepage
git init
git add .
git commit -m "Initial homepage"
git branch -M main
git remote add origin https://github.com/<你的用户名>/<你的用户名>.github.io.git
git push -u origin main
```

3. 仓库 → Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`，保存。
4. 一两分钟后访问 `https://<你的用户名>.github.io`。

仓库名若不是 `<用户名>.github.io`（比如叫 `homepage`），地址会变成 `https://<用户名>.github.io/homepage/`；文中全用相对路径，子目录部署同样可用。

---

## 二、填内容

待填处都标成 `<span class="todo">TODO: ...</span>`，浏览器里显示为**浅红底高亮**，一眼可见。

**全部填完后删掉 `style.css` 最后一行 `.todo` 规则**，高亮即消失。

### 头像

当前是 `images/profile.jpg`（380x491 竖版），由原始证件照 `images/IMG_7145.JPG` 裁出：按人物轮廓裁紧，上方留 4.6%、左右各 2%，四边不贴边框。CSS 再加 4px 内白边 + 1px 描边 + 8px 圆角，显示宽 120px。

想重裁（让脸更大或更小），调下面的 `TOP`（上方留白）与 `SIDE`（左右留白），在 `homepage/` 下运行该脚本。人物包围盒为 `x 131-864 / y 59-1000`：

    python3 -c "
    from PIL import Image, ImageOps
    src = ImageOps.exif_transpose(Image.open('images/IMG_7145.JPG')).convert('RGB')
    TOP, SIDE = 45, 15
    crop = src.crop((131-SIDE, 59-TOP, 864+SIDE, 1000))
    W = 380; H = round(W * crop.size[1] / crop.size[0])
    crop.resize((W, H), Image.LANCZOS).save('images/profile.jpg', quality=90, optimize=True)
    "

显示尺寸在 `style.css` 的 `.author__avatar img { max-width }`。

### 新增一篇论文

复制 `<!-- ===== 复制整个 .paper-box 来新增一篇 ===== -->` 标注的整个 `<div class="paper-box">`：

```html
<div class="paper-box">
  <div class="paper-box-image">
    <div>
      <div class="badge">ACL 2026</div>          <!-- 蓝色徽章，压在图左上角 -->
      <img src="images/my-paper.png" alt="">
    </div>
  </div>
  <div class="paper-box-text">
    <p class="paper-title"><a href="https://arxiv.org/abs/...">论文标题</a></p>
    <p class="paper-authors"><span class="me">Dingwei Chen</span>, Co-Author</p>
    <p class="paper-links"><a href="#">arXiv</a><a href="#">Code</a></p>
  </div>
</div>
```

- **缩略图**：`images/` 下，建议宽 400px 左右；CSS 会加投影（`box-shadow:3px 3px 6px #888`）。宽屏下图占 40%、文字占 60%，窄屏自动变成文字在上、图在下。
- **`.badge`**：会议与年份，蓝底白字 `#00369f`。想标 `Oral` 直接写进去。
- **`.me`**：自己的名字包在里面会自动加粗。共同一作用 `†`，并在该节末尾加一行说明。
- **没有图**：把整个 `<div class="paper-box-image">` 删掉，文字会自动占满。

### 删掉不需要的板块

News、Experience、Academic Services 任意一节没内容，就把它的 `<h1>` 和后面的 `<ul>` **一起删掉**。空板块比没有板块更显仓促。

---

## 三、改设计

配色与版式数值都取自参考站，集中在 `style.css` 顶部注释里：

| 用途 | 值 |
| --- | --- |
| 正文颜色 | `#494e52` |
| 正文字体 | `"Trebuchet MS", Helvetica, sans-serif`（系统字体，无需加载） |
| 根字号 / 行高 | `15px` / `1.5` |
| 链接 / 悬停 | `#224b8d` / `#142d55` |
| 分隔线 | `#f2f3f3`（标题下）、`#efefef`（论文条目间） |
| 次要文字 | `#7a8288`；页脚 `#9ba1a6` |
| 会议徽章 | 底 `#00369f`，白字 |
| 容器宽度 | `925px`，≥1280px 屏幕放宽到 `1280px` |
| 侧栏 / 正文 | 21% / 其余，侧栏 `position:sticky; top:6em` |

想换主色，改 `a{color}`、`a:hover{color}` 和 `.badge{background-color}` 三处即可。

**参考站是纯浅色、无深色模式**，本页保持一致。想加深色模式告诉我。

响应式断点 `900px`：以下侧栏变成顶部居中、论文条目变成单栏。

---

## 四、可选补充

- **CV**：PDF 放根目录命名 `cv.pdf`，导航栏那条链接即可用；不放就删掉导航里的 CV 项。
- **侧栏图标**：已接 Font Awesome 6（邮箱 / GitHub / 位置）与 Academicons（Google Scholar 官方标），从 cdnjs 加载、版本已固定。想省掉 Academicons 依赖，把 `ai ai-fw ai-google-scholar` 换成 `fa-solid fa-fw fa-graduation-cap` 并删掉对应的 `<link>`。
- **Google Scholar 引用徽章**：需要 Scholar ID（你的是 `MH2E99AAAAAJ`），要加告诉我。
- **自定义域名**：根目录加 `CNAME` 文件写域名，再到域名商处配 DNS。

---

## 五、标签页图标（favicon）

当前是 📚。四个文件在根目录：

```
favicon.svg          现代浏览器优先用这个（矢量，任意缩放清晰）
favicon-32.png       回退
favicon-16.png       回退
apple-touch-icon.png iOS 添加到主屏幕时用
```

**换 emoji**：改 `favicon.svg` 里 `<text>` 中间那个字符即可，PNG 回退可以不管（现代浏览器都优先读 SVG）。想连 PNG 一起换，在 `homepage/` 下跑：

```bash
python3 - << 'PY'
from PIL import Image, ImageDraw, ImageFont
EMOJI = "🤖"          # ← 改这里
f = ImageFont.truetype("/System/Library/Fonts/Apple Color Emoji.ttc", 160)
im = Image.new("RGBA", (160,160), (0,0,0,0))
ImageDraw.Draw(im).text((80,80), EMOJI, font=f, anchor="mm", embedded_color=True)
im.resize((180,180), Image.LANCZOS).save("apple-touch-icon.png")
im.resize((32,32),  Image.LANCZOS).save("favicon-32.png")
im.resize((16,16),  Image.LANCZOS).save("favicon-16.png")
PY
```

几个备选：📖 书 · 🤖 智能体 · 🧠 · 🔬 · 🐧

> 浏览器会长时间缓存 favicon。本地看不到变化时，硬刷新（⌘⇧R）或用无痕窗口。

---

## 六、版本存档

```
index.html / style.css   当前生效（minimal-light 风格）
v2-serif-timeline/       备选设计：Newsreader 衬线 + 左栏时间轴（未采用）
```

`v2-serif-timeline/` 里的 `images/` 与 favicon 是指向上级的软链接，直接打开其中的 `index.html` 即可预览。想换用该版本：把里面的 `index.html`、`style.css` 复制到根目录覆盖 —— 但注意其正文内容停留在旧版，需要重新同步。不打算用就整个删掉。
