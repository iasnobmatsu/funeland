---
title: How to Build Your Own Blog Using Hexo and GitHub Pages (Mac, 2022)
date: 2022-11-07 14:51:09
tags: [blog, hexo]
categories: [tools]
mathjax: true
---

### Before We Begin

Required tools: [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [node.js](https://nodejs.org/en/). These should be downloaded first. You can use `node -v` and `git --version` to check if you have these installed. 


### Install Hexo

Hexo can be installed using node.js. After opening the command line tool, run `npm install -g hexo-cli`.
See the official documentation on how to install hexo [here](https://hexo.io/docs/).

### Start A Blog

To start a new blog with hexo is simple. After going to the path you want to place your blog folder, run `hexo init [blog]` with `[blog]` being the name for your folder. Then, `cd [blog]` to enter the folder and run `npm install` to install all dependencies.

Congratulations, you now have initialized your blog! To take a look at your own blog, type `hexo s` or `hexo server` in the blog directory, and then go to the corresponding localhost:xxx site to view your blog.

### Using Hexo Themes

We all know that looks matter for a blog. Luckily, with hexo, there is an abundance of [themes](https://hexo.io/themes/) you can choose from. I used the [cactus theme](https://github.com/probberechts/hexo-theme-cactus). Below, I will show you how to use the theme you like. 

After figuring out which theme I want to use (cactus in this case), I downloaded the theme files using `git clone https://github.com/probberechts/hexo-theme-cactus.git themes/cactus` on the command line. This should be done within your blog directory. If successful, you will see that within the `themes/` folder there is a `cactus` folder containing all theme files. 

Then, within the `_config.yml` file in the blog root directory, you can change `theme: landscape` (the default) to `theme: cactus`. Now, if you refresh the server, you shall see that your blog is using the brand new theme we just installed!

### Write Blog Posts

To add a new post, you can type `hexo new post [post name]` on the command line, which will automatically add a `[postname.md]` file under the `/source/_posts/` directory. You can edit the markdown file to write your blog post. 

### Add Images in Blog

First, a plugin is needed. Run `npm install hexo-image-link --save` on command line to install the image link plugin. To include images in your writing, you can add a folder with the same name as your blog in the `/source/_posts` directory. For example, if I'm writing a post called `test.md` then I would need a `test` asset folder for the images. After putting images in the corresponding asset folder, for example, `test_img.png`, simply use the markdown syntax to include the image like `[image title](test_img.png)`. Note that if you want the asset folder to be automatically generated every time you add a new blog post, simply change `post_asset_folder: false` to `post_asset_folder: true` in the `_config.yml` file. 

### Adding Latex Compatibility

For Latex to work properly, I tried several tutorials online and neither of them have worked to perfection. The method below worked okay for me:

1. `npm install hexo-math --save`
2. `npm install hexo-renderer-kra --save`
3. `npm install hexo-renderer-kramed --save`
4. `npm install hexo-filter-mathjax --save`
5.  add `plugins: hexo-math` in `_config.yml`
6. add this to  `_config.yml`
```{bash}
mathjax:
  enable: true
  per_page: true
```
7. in the header of your markdown file, add `mathjax: true`

Now latex should work! so I can type something like $\alpha+\frac{1}{\beta}$.

I could not figure out how to make latex blocks render though (those with two `$`). Need some help with this. 

### Adding Categories and Tags

Tags and categories are my favorite hexo features. In your blog post header, things like 

```
tags: [blog, hexo, github]
categories: [miscellaneous]
```
could be added, and this will automatically generate three tags (blog, hexo, github) and one category (miscellaneous) as shown below. You can click on the tag/category to see posts corresponding to them.

![tags](tags.png)

However, this poses an incovenience. That is, we cannot see all available tags or categories. To make a webpage containing all tags, run `hexo new page tags` which will generate a `tags` folder under `source`. Within this folder, there is a `index.md` file which controls the tags page. Change `type:` to `type: tags` so that this page will show all available tags. To view this page, re-route to the `/tags` path. So if I'm testing my blog on `http://localhost:4000/`, this path would be `http://localhost:4000/tags/`. 

The categories page can be made similarly.


### Customizing Your Blog

Many themes such as $cactus$ and $next$ provide options for you to customize how your website looks. This can typically be done in the `_config.yml` file. However, if you need to further fine tune how your blog looks, then this might involve some changes to files under the `\themes` folder. For example, I changed font color, logo, and some text displays for my blog from the cactus theme. 

### Deploy to Github

1. In github, create a new repository named [your username].github.io
2. In the blog root directory, run `npm install hexo-deployer-git --save`
3. Add the following to `_config.yml` (learnt from [here](https://medium.com/techtofreedom/3-steps-to-build-your-static-website-with-hexo-and-github-pages-9bc9b26a24c2))
```
deploy:
  type: git
  repo: [your repository]
  branch: main
  message: [message]
```
4. Push your files to the repository 
5. In your repository, find settings -> Pages -> Your GitHub Pages site is currently being built from the XXX branch, choose the `gh-pages` branch.

Done!

Now you can access your blog via `https://[your username].github.io`. 







