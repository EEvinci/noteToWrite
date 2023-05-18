# Hexo博客搭建 —— Next主题配置

[TOC]

## 配置文件区分

### 站点配置文件

站点配置文件在hexo根目录下

![image-20230501164711002](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230501164711002.png)

可以看到上面还有一个`_config.next.yml`

这是因为配置了主题, 所以要在hexo的根目录下引入将主题的`config`配置文件, 使得主题的配置文件生效

该文件的内容和下面的主题配置文件中的内容相同(就是从主题下的`_config.yml`改名复制过去的)

### 主题配置文件

![image-20230501164859361](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230501164859361.png)

在主题下有一个单独的`_config.yml`, 是该主题的配置文件

但是在修改主题配置文件时，我们修改的是出于hexo根目录下的`_config.next.yml`，这是为了避免将来主题更新时，由于新版主题自带的默认的初始的`_config.yml`导致的配置文件冲突并覆盖因此造成自定义的原配置文件的丢失问题.

> 当你直接在主题文件夹内修改 `_config.yml` 文件并且在将来更新主题时，新版本的主题可能会包含一个新的 `_config.yml` 文件。在更新过程中，新版本的 `_config.yml` 文件可能会覆盖你修改过的 `_config.yml` 文件，导致你之前所做的自定义设置丢失。
>
> 为了避免这种情况，建议你将 Next 主题的 `_config.yml` 文件拷贝到 Hexo 博客根目录，并重命名为 `_config.next.yml`。然后在这个新文件中进行自定义设置。这样，即使你更新了主题，你的自定义设置仍会保存在根目录下的 `_config.next.yml` 文件中，不会被新版本的配置文件覆盖。

## 配置文件描述简称

对站点配置文件的修改会在二级标题后附加**site**

对主题配置文件的修改会在二级标题后附加**next**

## 引入next主题-site

![image-20230501165256918](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230501165256918.png)

## next主题页面模式设置-next

![image-20230501164552268](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230501164552268.png)

> 1. Muse：这是一个简洁、纯净的主题，它拥有一个类似于单栏布局的设计。这种布局使得博客的内容成为了页面的核心。Muse 模式适用于那些喜欢简约风格、注重内容呈现的用户。
> 2. Mist：Mist 模式相对于 Muse 更加丰富，它采用了双栏布局。在页面的右侧，可以看到一个侧边栏，用于展示博客的其他信息（如分类、标签、归档等）。Mist 模式适用于那些需要更多功能并希望在博客中呈现更多信息的用户。
> 3. Pisces：Pisces 模式采用了一种双栏布局，但与 Mist 不同，它在页面的两侧都有侧边栏。这种布局在视觉上更加对称，适用于那些喜欢对称风格并希望在博客中展示更多信息的用户。
> 4. Gemini：Gemini 模式也采用了双栏布局，但它的侧边栏设计更加紧凑，这使得页面在视觉上更加集中。Gemini 模式适用于那些喜欢简约风格但又需要一些额外功能的用户。

## 设置首页不显示全文-site

```yml
excerpt accurately.
auto_excerpt:
enable: true
length: 150
```

在原配置文件中没有

需要额外添加

![image-20230501170358210](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230501170358210.png)

## 设置博客文章持久化连接-site

![image-20230501171628072](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230501171628072.png)

将原来的持久化链接格式注释掉，然后设置新的格式为 `post/:abbrlink.html`。

需要确保你的博客文章链接格式是唯一的。在这种情况下，新的链接格式将替代原来的链接格式。

### 下载插件

Hexo 的 `hexo-abbrlink` 插件. 

这个插件用于生成基于文章内容的唯一缩写链接（abbrlink）。这将确保文章链接是唯一的，从而避免了链接冲突的问题。

安装 hexo-abbrlink 插件，在 Hexo 博客根目录下运行以下命令：

```shell
npm install hexo-abbrlink --save
```

安装完成后，需要`在 Hexo 配置文件（_config.yml）中配置插件的相关设置:

```yaml
abbrlink:
  alg: crc32
  rep: hex
```

这里的 `alg` 表示选择的算法（如 crc32），`rep` 表示缩写链接的表示形式（如十六进制，hex）。

### URL Setting

> 1. `url`: 这个设置是你的网站的基本 URL。你需要在这里设置你的博客网站的域名，这样 Hexo 生成的链接才能正确指向你的网站。如果你使用 GitHub Pages，你的 URL 应该设置为 `https://username.github.io/project`。
>
>    以你的 GitHub 仓库为例，你的 URL 应该设置为：
>
>    ```
>    url: https://eevinci.github.io
>    ```
>    
>    > 当你使用名为 `username.github.io` 的仓库时，你可以利用 GitHub Pages 服务来托管你的静态网站或博客。在这种情况下，你需要在 Hexo 配置文件（**_config.yml**）中设置 `url` 为 `https://username.github.io`（例如，`https://eevinci.github.io`）。
>    >
>    > 然而，如果你没有使用这种特殊命名的仓库，那么你的仓库仅仅是一个用于托管代码的平台。要想将博客部署到互联网上，你需要使用其他的部署服务，如 **Netlify**、**Vercel** 等。在这种情况下，你应该在 Hexo 配置文件中设置 `url` 为你的自定义域名，或者使用部署服务分配给你的临时域名（例如，`https://your-project-name.netlify.app`）。这将确保 Hexo 生成的链接能正确指向你的博客。
>    >
>    > 所以，根据你的实际部署情况，你需要在 `_config.yml` 文件中正确设置 `url`。如果你使用 GitHub Pages，请设置为 `https://username.github.io`；如果你使用其他部署服务，请设置为相应的域名。
>    
> 2. `pretty_urls`: 这是一个用于**配置 URL 格式**的属性。它包含了两个子属性：
>
>    - `trailing_index`: 如果设置为 `false`，则会从链接中删除尾部的 'index.html'。例如，`https://yourdomain.com/about/index.html` 变为 `https://yourdomain.com/about/`。
>    - `trailing_html`: 如果设置为 `false`，则会从链接中删除尾部的 '.html'。例如，`https://yourdomain.com/about.html` 变为 `https://yourdomain.com/about`。
>
>    这些属性可以让博客链接看起来更简洁、易于阅读。
>    
>    可以根据自己的喜好设置这些属性。