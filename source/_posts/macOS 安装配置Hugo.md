[Hugo](https://github.com/gohugoio/hugo)
[Install Hugo](https://gohugo.io/getting-started/installing/)[零基础使用Hugo和GitHub Pages创建自己的博客](https://jimmysong.io/posts/building-github-pages-with-hugo/)

`brew install hugo`


下载主题

[Beautiful Hugo github](https://github.com/halogenica/beautifulhugo)|[Beautiful Hugo](https://themes.gohugo.io/beautifulhugo/)

`git clone https://github.com/halogenica/beautifulhugo.git themes/beautifulhugo`


`hugo server -t beautifulhugo --buildDrafts`

-t参数的意思是使用beautifulhugo主题渲染我们的页面，注意到about.md目前是作为草稿，即draft参数设置为true，运行Hugo时要加上--buildDrafts参数才会生成被标记为草稿的页面。 在浏览器输入localhost:1313，就可以看到我们刚刚创建的页面。

注意观察当前目录下多了一个文件夹public/，这里面是Hugo生成的整个静态网站，如果使用Github pages来作为博客的Host，你只需要将public/里的文件上传就可以，这相当于是Hugo的输出。