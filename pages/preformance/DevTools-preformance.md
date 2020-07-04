<style>

  .note {
    padding: 16px 24px;
    font-size: 14px;
  }
  .promiry {
    background-color: #e1f5fe;
    color: #01579b;
  }
  .warn {
    background: #feefe3;
    color: #bf360c;
  }
  .icon {
    display: inline!import;
    margin: 0!important;
    box-shadow: none;
  }
</style>

# 前言

这篇文章Google开发者中[DevTools](chrome的开发者工具，由几个选项卡工具组成。 "Devtools是什么")文档Performance部分的英文译文，如有翻译不对的地方，希望大家指出  

原文：[https://developers.google.com/web/tools/chrome-devtools/evaluate-performance](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance)  
作者： Kayce Basques  
译者： Timwk

*注：未经许可，请勿转载*

# 正文

***注：可以查看[优化网站速度](https://developers.google.com/web/tools/chrome-devtools/speed/get-started)以了解如何使您的页面加载速度更快。***

运行性能（Runtime performance）是页面运行（而不是加载）时的性能。 本教程将教您如何使用“Chrome DevTools”的性能选项卡(performance)来分析运行性能。 在[RAIL(response, animation, idle, load)](https://web.dev/rail/)模型方面，您在本教程中学习的技能对于分析页面的响应，动画和空闲阶段很有用。

<div class="note warn">
警告：本教程基于Chrome59。如果您使用其他版本的Chrome，则DevTools的UI和功能可能会有所不同。 检查chrome：// help，查看您正在运行的Chrome版本。
</div>

## 教程开始

在本次教程中，你在页面中打开 **DevTools**，并使用**Performance** 选项卡来查找页面的性能瓶颈
  
  1. 在隐身模式中打开 **Google Chrome**。隐身模式能够使 **Chrome** 进入纯净状态，例如：如果您安装了许多浏览器扩展，这些扩展可能会干扰性能评估，得不到准确的数据；
  2. 在隐身窗口中打开下面页面。这是该教程的演示页面。这个页面中显示了一群上下移动的小蓝色方块；  
  [https://googlechrome.github.io/devtools-samples/jank/](https://googlechrome.github.io/devtools-samples/jank/)
  3. 使用快捷键 ***Command+Option+I***(Mac) 或 ***Control+Shift+I***(Windows,Linux) 打开`DevTools` ,效果如下图：
  ˆ![图1](https://static01.imgkr.com/temp/94bce80b79ef47fbad0a2f8830354564.png)
  **图1** 页面在左边，DevTools在右边
  
  <div class="note promiry">
  注意：对于后续其他的屏幕截图，`DevTools`将作为<a url='https://developers.google.com/web/tools/chrome-devtools/customize#placement'>单独的窗口</a>出现，以便你能更好的查看内容
  </div>
  
## 模拟移动CPU

与台式电脑和笔记本相比，移动设备的CPU的性能要低很多。所以当你分析页面，一般使用CPU限制来模拟页面在移动设备上的表现；

1. 在**Devtools**中，单击”性能“选项卡；
2. 确定启动 **Screenshots** 复选框；
3. 单击 **Capture Setting**<img class="icon" src="https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/capture-settings.png" />。**DevTools** 会展开跟性能分析时环境相关的设置。
4. 对于**CPU**选项，选择**2x slowdown**。DevTools会限制你的CPU,使其比正常时慢2倍
![](https://static01.imgkr.com/temp/35ef901b4eb64ada8abfd0d9a65369c7.svg)
**图2** CPU 节流设置，蓝色框处

<div class="note promiry">
  注意：在测试其他页面时，如果要确保它们在低端移动设备上可以正常工作，请将CPU限制设置为20倍速度减慢。 此演示无法与20倍的减速一起很好地工作，因此仅将2倍的减速用于教学目的。<br/>  
译者注：目前的手机cpu性能已远超之前，目前chrome中只能设置4x\6x
</div>

## 演示设置

因为很难创建一个使所有用户都表现一致的运行性能[演示页面](因为用户的设备配置和performance选项版配置的不同。 "无法得到表现一致的演示（译者注）")。接下来你需要自定义演示，以确保在你的条件下，你的体验与本教程中看到的屏幕截图和说明相对一致；

1. 单击“Add 10” 按钮，知道蓝色方块的移动速度明显比初始时慢。在性能好的计算机上，可能需要约20次的点击

2. 单击“Optimize”（优化）按钮，蓝色方块能移动得更快更稳

<div class="note promiry">
  注意：如果你看不到优化和未优化版之间的明显差异，请尝试单击“Subtract 10”按钮几次，然后重试上面操作。如果添加过多的蓝色方块，则只会使CPU达到最大值，而不会看到两个版本的结果有太大的差异；
</div>

3. 单击 ”Un-Optimize“按钮。蓝色方块的移动速度会变慢和页面变得卡顿

## 记录运行性能

当您运行页面的优化版本时，蓝色方块移动得更快。 这是为什么？ 两种版本都应在相同的时间内将每个正方形移动相同的空间。 在**performance**选项卡中进行录制，可以了解如何检测未优化版本中的性能瓶颈。

1. 在**DevTools**中，单击“Record”。**DevTools** 捕获页面运行时的性能指标。


![](https://static01.imgkr.com/temp/f908b88cd1ce4e6e9ceb9ebe75b9b491.png)
**图3**：页面分析

2. 等几秒钟。
3. 点击 ”Stop“ 按钮，**DevTools** 停止记录，处理数据，然后在**performance**选项卡上显示最终结果
![](https://static01.imgkr.com/temp/224dd3a769354e799842fe53fb147f43.png)
**图4**：分析结果

wow, 这是数据量也太大了吧。但不用担心，接下来你就会知道这些数据的含义的；


## 结果分析

记录了页面的性能后，就可以衡量页面的性能有多差，并找出原因。

## 分析每秒帧数（Frames Per Second,简称：FPS)

衡量动画性能的主要指标是每秒帧数（FPS）。 [当动画以60 FPS运行时，用户看着是流畅的](与人眼识别有关系 "为什么是60 FPS")。

1. 查看**FPS**表，如果你在图标的上方看到红色条，则表示页面帧数很低，会影响用户的使用体验（译者：会让用户觉得有卡顿出现）
![](https://static01.imgkr.com/temp/eeea679819984fe5a3403b1e262829c9.svg)
**图5**：蓝框标识的是 FPS 图表

2. 在 FPS 图表下方，你会看到CPU图标。CPU图表中的颜色与**Performance**选项卡底部的**Summary**选项卡的颜色对应。CPU图表中，颜色填充的高度越高则表示使用CPU的资源越多。当你看到CPU长时间处于满负荷的状态，就需要寻找减少使用CPU的方法；
ˆ![](https://static01.imgkr.com/temp/9a4ae016d99046088438434c85fd217f.svg)

**图6**：蓝框标识CPU图标和**Summary**选项卡

3. 将鼠标悬停在FPS，CPU或NET图表上。 **DevTools**将显示该时间点的页面截图。 左右移动鼠标以显示记录。 这称为 scrubbing(译者：这里暂时没找到合适的翻译)，对于手动分析动画的播放很有用。
![](https://static01.imgkr.com/temp/8b05f7caea0645a48ee4b37d7eae063d.png)
**图7**：查看记录中约2000ms处页面的屏幕截图

4. 在“Frames”(帧)部分，将鼠标悬停在某个绿色的方块上，**DevTools** 会显示特定帧的FPS,帧数可能远远达不到 60 FPS 的目标；

![](https://static01.imgkr.com/temp/aa57d61ea4c34342a437bc95fef7c198.png)

**图8**：悬停在帧上

在这个演示上，我们能很明显的看出页面效果不好。但在实际情况中，可能不会很明显，因此使用这些工具能很方便的进行检测。

## 额外：打开 FPS 仪表

另一个方便的工具是FPS测量仪，它可以在页面运行时提供FPS的实时估算。

1. 按Command + Shift + P（Mac）或Control + Shift + P（Windows，Linux）打开“命令”菜单。
2. 在输入栏输入“Rendering”，然后选择“Show Rendering”。
3. 在**Rendering**选项卡中，选中**FPS Meter**, 会有一个新的悬浮层显示在视图的右上角（MAC是左上角）。
![](https://static01.imgkr.com/temp/a81629378a7e4477b140ca97f271607b.png)

**图9**：FPS 面板

4. 取消 FPS Meter， 然后按ESC键关闭**Rendering**选项卡，在本教程你将用不到这个面板。

## 寻找瓶颈

现在，您已经测量并验证了动画效果不佳，接下来要回答的问题是：为什么？

1. 注意**summary**选项卡。 如果未选择任何事件，则此选项卡显示活动的分类。 该页面花费了大部分时间进行渲染。 由于提高性能是减少浏览器工作量的艺术，因此您的目标是减少花费在进行渲染工作上的时间。

![](https://static01.imgkr.com/temp/9c28958efcd34c6cbcc28c4792cf1e04.svg)
**图10**：蓝色框是**Summary**选项卡

2. 展开“Main”部分。 **DevTools** 向您显示了主线程随时间变化的活动图表。 x轴表示一段时间内的记录。 每个条形代表一个事件。 宽条表示该事件花费了更长的时间。 y轴表示调用堆栈。 当您看到事件相互叠加时，表示较高的事件调用较低的事件。
![](https://static01.imgkr.com/temp/9dfd7578afee47e28b72fa2c89f16987.svg)

**图11**：蓝色框是“Main”块

3. 记录中有很多数据。 通过单击，按住并在“概述”上拖动鼠标来放大单个**Animation Frame Fired**事件，该概述是包括FPS，CPU和NET图表的部分。 “Main”块和“summary”选项卡仅显示记录中所选部分的信息。
![](https://static01.imgkr.com/temp/36a7273e06da4d4a8c93edb7015f94ee.png)
**图12**：放大单个`Animation Frame Fired`事件

<div class="note promiry">
  注意：另一种缩放方法是通过单击"Main"块的背景或选择一个事件来聚焦观察，然后按W，A，S和D键（译者：W是放大，S是缩小，S左移，D右移）。
</div>

4. 请注意“Animation Frame Fired”事件右上角的红色三角形。但你看到有红色三角形出现的时候，表明该事件存在问题。

<div class="note promiry">
  注意：当requestAnimationFrame()回调执行时，会触发“Animation Frame Fired ”事件的触发
</div>

5. 点击`Animation Frame Fired`事件时，**Summary**选项卡会显示该事件的相关信息。注意`reveal`链接，点击链接， **DevTools**会突出显示启动该`Animation Frame Fired`事件的事件。同时还要注意`app.js:94`链接，点击会跳到源代码中的相关行。

![](https://static01.imgkr.com/temp/d8e1cf175dc84b20aeaced9d621d9c2d.png)

**图12**：关于`Animation Frame Fired`事件的更多消息

<div class="note promiry">
  注意：选择事件后，可以使用方向键选择该事件旁边的事件
</div>

6. 在`app.update`事件下方，有一堆紫色事件。如果将其放宽，会看到几乎每个紫色事件上都有一个红色三角形。现在单击其中一个紫色的`Layout`事件。查看**DevTools**中的**Summary**选项卡提供的信息，会发现确实存在强制重排（reflows)的警告(也就是强制布局的警告)

7. 在**Summary**选项卡中，单击`Layout Forced`下的`app.js:70`链接，**DevTools**会切换到触发强制布局的代码行
![](https://static01.imgkr.com/temp/d77865a64e624b32b86b0085bc4cc8f5.png)

**图13**：触发强制布局的代码行

<div class="note promiry">
  注意： 此代码的问题是: 在每帧动画帧中，它都会修改正方形的样式，然后查询页面上每个正方形的位置。由于样式已被修改，浏览器就不知道正方形的位置是否有更改，所以就需要重新布局正方形才能计算其位置。可以查看[ Use transform and opacity changes for animations ](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance)学习更多相关知识
</div>

做得很好，所以花费了一些时间，但是现在你已经有分析运行性能的坚实基础了。

## 额外：分析优化版本

单击演示页面上的`Optimize`按钮，启动优化了代码的版本，然后使用刚学习的流程和工具进行性能记录，然后分析结果。从改进的帧率到“Main”块火焰图中事件的减少，你看到优化版本的应用做了更少的工作，从而获得更好的性能；

<div class="note promiry">
  注意： 这个“优化”版本也不是最好的，因为它仍然操作每个正方形的 top 属性。更好的方法是只操作影响合成的属性。有关更多信息，请参见<a url="https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count#use_transform_and_opacity_changes_for_animations" >Use transform and opacity changes for animations </a>
</div>

## 下一步

*RAIL*模型是了解性能的基础。该模型告诉你，对用户而言，哪些是最重要的性能指标。有关更多信息，请查看[Measure performance with the RAIL model](https://web.dev/rail/)

更多的实践能让你对**Performance**选项卡更加的熟悉。请尝试对你自己的页面进行分析，并分析结果。如果你对你的结果有任何疑问，[可以在Stack Overflow发布一个标有`google-chrome-devtools`标签的问题](https://stackoverflow.com/questions/ask?tags=google-chrome-devtools)。如果可以，请附上屏幕截图或指向可复制页面的链接

要真正掌握运行时性能，你必须学习浏览器如何将HTML、CSS和JS转换为屏幕上像素点。可以从[ Rendering Performance Overview](https://developers.google.com/web/fundamentals/performance/rendering)开始学习。[The Anatomy Of A Frame](https://aerotwist.com/blog/the-anatomy-of-a-frame/)这篇文章深入讨论了更多细节。

最后，有许多方法可以改善运行时性能。 本教程重点介绍了一个特定的动画瓶颈，以使您可以重点关注**Performance**面板，但这只是您可能遇到的许多瓶颈之一。 渲染性能系列的其余部分提供了许多改善运行时性能各个方面的好技巧，例如：

  * [Optimizing JS Execution](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution)
  * [Reduce The Scope And Complexity Of Style Calculations](https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations)
  * [Avoid Large, Complex Layouts And Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing)
  * [Simplify Paint Complexity And Reduce Paint Areas](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)
  * [Stick To Compositor-Only Properties And Manage Layer Count](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)
  * [Debounce Your Input Handlers](https://developers.google.com/web/fundamentals/performance/rendering/debounce-your-input-handlers)
