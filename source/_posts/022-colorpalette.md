---
title: Jojo themed R Color Palette
date: 2022-12-05
updated: 2022-12-15
tags: [R, anime]
categories: [projects]
description:  R palette based on Jojo's Bizzare Adventure characters compatible with ggplot2.
top: true
cover: unnamed-chunk-2-1.png
---


Inspired by [Harry Potter color palette](https://github.com/aljrico/harrypotter) abd [rock album color palette](https://github.com/johnmackintosh/rockthemes), this is a similar project based on Jojo's Bizzare Adventure series. Currently, color palettes for Jojos from part 1 to 6 are implemented. Palettes based on stands and other characters will be added.


#### Install the JJBApalette package

```R
    library(devtools)
    install_github("iasnobmatsu/JJBApalette")
    library(JJBApalette)
    library(ggplot2)
```

#### View colors in the jolyne palette

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



