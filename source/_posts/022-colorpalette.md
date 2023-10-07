---
title: Jojo themed R Color Palette
date: 2022-12-05
updated: 2022-12-15
tags: [R, anime]
categories: [projects]
description: Inspired by Harry Potter color palettes, this is an R palette based on Jojo's Bizzare Adventure characters.
top: true
---

## 2022-12-15

几天前完成了1-6代JoJo的色盘。以后有机会加别的人物以及替身使者。

## 2022-12-05

起因是今天有人给我看了[Harry Potter color palette](https://github.com/aljrico/harrypotter). 然后又发现了一个[摇滚乐album的color palette](https://github.com/johnmackintosh/rockthemes). 

JOJO主题的color palette的话感觉可以以人物和替身为单位来做。

## 2022-12-07

今天用徐伦的主题色做了一个sample.

Install the JJBApalette package

```R
    library(devtools)
    install_github("iasnobmatsu/JJBApalette")
    library(JJBApalette)
    library(ggplot2)
```

View colors in the jolyne palette

```R
    jolyne_colors = data.frame(color = as.vector(jolyne_palette()),
                               val = rep(1, length(jolyne_palette())))
    ggplot(data = jolyne_colors, aes(x = color, y = val, fill = color)) + 
      geom_bar(stat="identity") +
      scale_fill_jolyne("discrete") + theme_light()
```
<img src="unnamed-chunk-2-1.png" width=400px>

```R
    data(mtcars)
    ggplot(data = mtcars, aes(x = wt, y = mpg)) + geom_point(aes(color=factor(cyl))) + 
      scale_color_jolyne("discrete") + theme_light()
```

<img src="unnamed-chunk-2-2.png" width=400px>

```R
    ggplot(data = mtcars, aes(x = wt, y = mpg)) + geom_point(aes(color=hp)) + 
      scale_color_jolyne("continuous") + theme_light()
```
<img src="unnamed-chunk-2-3.png" width=400px>



