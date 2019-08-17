---
title: 谷歌浏览器渲染原理
date: 2017-3-02 16:29:44
categories:
  - 前端开发
  - 运行环境
  - 标准浏览器
  - 2.谷歌浏览器渲染原理
tags:
  - 3.javaScript
- 谷歌浏览器渲染原理
---

### 页面渲染原理

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染.png)



一个页面展示在用户面前，简单来说，会经历以上5个步骤。我们可以把上面这个图称为`像素管道`

- JavaScript：执行js逻辑，修改DOM,修改CSS等。

- Style:计算样式。

- Layout:在知道对一个元素应用那些规则之后，浏览器即可开始计算它要占据的空间大小及其在屏幕的位置。这个步骤，就是我们常说的重排。

- Paint:绘制是填充像素的过程。它涉及绘出文本、颜色、图像、边框和阴影，基本上包括元素的每个可视部分。绘制一般是在多个表面（通常称为层）上完成的。这个步骤，即使我们常说的重绘。

- Composite:渲染层合并，由上一步可知，对页面中DOM元素的绘制是在多个层上进行的。在每个层上完成绘制过程之后，浏览器会将所有层按照合理的顺序合并成一个图层，然后显示在屏幕上。

  在浏览器器中，页面的渲染由浏览器的渲染进程完成，而渲染进程中，包含了主线程，worker线程，Compositer线程，Raster线程。上述像素管道的5个过程总，前4个过程，都由主线程完成，最后一个步骤，主要有Raster线程、Compositer线程完成。

#### JavaScript、Style、Layout

像素管道中的前三个步骤，JavaScript、Style两个步骤如下：

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染2.png)

接着是Layout,浏览器遍历render tree的每一个节点，计算其确切的位置和大小。最终形成一个Layout Tree.

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染3.png)

#### Paint

在Paint之前，浏览器会根据Layout Tree，确切需要绘制的对象的层级，我们可以把这个层级叫做`渲染层`，最终生成Layout Tree。这个阶段被称作：`Update Layer Tree`

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染4.png)

在Paint这个阶段，浏览器会根据`Layout Tree`,生成Paint Records.

Paint Records 就是描述先画什么，再画什么的记录，跟我们写canvas代码时很像。Paint Records是根据渲染层划分的。

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染5.png)

尽管生成了Paint Records,真正的绘制并不是Paint这个阶段完成的，而是在Composite阶段由Raster线程完成的。

#### Composite

经过之前的几个步骤，浏览器主线程已经将页面的内容分成了若干渲染层。为了提升性能，某些特定的渲染层，会被提升为`合成层`。我们可以通过下面两个css属性，将某个元素强制提升为合成层：

```
will-change:transform;
//或者
transform:translateZ(0)
```

主线程在处理完所有的数据后，会把数据提交到Compositer线程。Compositer线程会利用Raster线程来做光栅处理，并将处理好的内容存入内存中。随着Compositer线程完成渲染层合成操作，扔给GPU,页面最终被渲染到屏幕上。

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染6.png)

可以通过Chrome开发者工具中的Layer来查看合成层。

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染7.png)

#### 其他像素管道

上文中的像素管道共有5个步骤。不一定每帧都总是会经过管道每个部分的处理。实际上，不管是使用JavaScript、CSS还是网络动画，在实现视觉变化时，管道针对指定帧的运行还有其他两种方式：

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染8.png)

![https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/%E6%B8%B2%E6%9F%932.png](https://zhoubichuan.github.io/Note-Frontend/4.run/1.browser/1.loadingRender/1.render/渲染9.png)

- 第一种就是我们所说的页面没有进行重排，值进行了重绘；
- 第二种就是页面即没有进行重排，也没有进行重绘
- 最后的这种运行方式开销最小，适合于页面上的动画效果。