---
home: true
# heroImage: /img/web.png
heroText: 爱写bug的小邓程序员
tagline: Web前端技术博客，积跬步以至千里，致敬每个爱学习的你。
actionText: 立刻进入 →
actionLink: /web/
bannerBg: 'none'

features: # 可选的
  - title: 前端
    details: JavaScript、ES6、Vue框架等前端技术
    link: /web/ # 可选
    imgUrl: /img/web.png # 可选
  - title: 后端
    details: 后端相关技术
    link: /backend/
    imgUrl: /img/ui.png
  - title: 扩展
    details: 技术文档、教程、技巧、总结等文章
    link: /extend/
    imgUrl: /img/other.png

# 文章列表显示方式: detailed 默认，显示详细版文章列表（包括作者、分类、标签、摘要、分页等）| simple => 显示简约版文章列表（仅标题和日期）| none 不显示文章列表
# postList: detailed
# simplePostListLength: 10 # 简约版文章列表显示的文章数量，默认10。（仅在postList设置为simple时生效）
# hideRightBar: true # 是否隐藏右侧边栏
---

<style>
:root {
  --vp-home-hero-name-color: transparent;
  --vp-home-hero-name-background: -webkit-linear-gradient(120deg, #bd34fe 30%, #41d1ff);

  --vp-home-hero-image-background-image: linear-gradient(-45deg, #bd34fe 50%, #47caff 50%);
  --vp-home-hero-image-filter: blur(44px);
}
.banner {
  background: rgb(217,231,255)!important;
  background: linear-gradient(180deg, rgba(217,231,255,1) 0%, rgba(227,255,231,0.6) 70%,  rgba(236,249,238, 0.5) 80%, rgba(244, 244, 244, 0.5) 90%)!important;
  backdrop-filter: blur(var(--vp-home-hero-image-filter));
}
#main-title {
  color: transparent;
  background: var(--vp-home-hero-name-background);
  -webkit-background-clip: text; /* WebKit浏览器的前缀 */
  background-clip: text;
}
#main-title:before{
  content: "🚀";
  color: black;
}

.icon{
  font-size: 30px;
}

@media (min-width: 640px) {
  :root {
    --vp-home-hero-image-filter: blur(56px);
  }
}

@media (min-width: 960px) {
  :root {
    --vp-home-hero-image-filter: blur(68px);
  }
}
</style>