title: es6-proxy
date: 2018-12-03 13:34:38
tags:
---
2014年谷歌搞了个实验项目 Sky ，号称基于Dart来开发现代化移动优先的高性能 跨平台App，帧率可以达到120及以上，2015年10月 Sky 改名为Flutter，并发布了官方网站flutter.io，谷歌内部开始用Flutter开发实际项目，2018年发展迅速开始大火了。可以看出Flutter跟RN差不多是时期的产品，Flutter在应用层玩法也是受React很多的启发，二者UI工作原理不同，RN用的系统SDK的UI，Flutter是自绘制的UI，操作UI不需要bridge来进行通信 确保了性能 。更多Flutter原理 https://www.zhihu.com/question/50156415
Flutter创始人Eric Seidel 十多年来一直从事Chrome 的开发， 最初目的是提高web应用程序的体验 ，基于chrome代码移除了很多功能，甚至移除渲染web程序的能力。，，跑了些benchmarks发现性能提升20倍。为了能够做更多的事情，又增加了很多功能，经过三次大的调整之后就成了现在的Flutter。FLutter也致力于提供高性能移动端跨平台App的开发体验。FLutter主要优点如下（其实最主要的可以概括为 简单、高性能、全平台的UI开发体验以及谷歌的大力支持）

## 主要优点
- 跨平台 基于Skia绘图引擎实现全平台UI绘制 android ios web linux mac windows 也是作为谷歌未来系统fuchsia的御用UI。完全是重新定义新标准的节奏。并且全平台都是谷歌官方自己出的方案（RN官方只是支持ios 和android。桌面和web都是社区给的方案）FLutter也是谷歌内部多个Team的合作的作品（Dart、 Flutter、 chrome等）
- HotReload 改了代码秒见效果。
- 性能 致力于提供60fps的帧率体验
- Dart。 生来背着灭掉JS，替换Java的使命，前几年一直不是很火，最近谷歌也很重视。还专门为FLutter 进行Dart的优化，AOT模式下编译为可执行的代码，执行时不需要Dart虚拟机。Dart语法简洁，比Java和OC写起来简单太多。异步单线程完全是前端的玩法。
- React style。 Flutter官方也表明过其设计思想最初也是受Reac个启发，一切都是组件，没有像android ios 那些些activity fragment 杂七杂八的概念，写应用的模式与React几乎是一模一样，写的多了感觉就是用Dart写React。Flex布局思想可以直接用、React的Component 和 PureComponent，对应Flutter里面有StateFullWidget 和 StateLessWidget，Context 对应Flutter中的 inheritWidget，状态管理redux 对应Flutter_Redux，React里面可以用RxJS，Flutter里面可以用RxDart，都是Reactive UI风格、都是基于虚拟DOM实现UI更新，甚至React新出的Hooks,在Flutter 里面也有了第三方的支持。相比React Native 。Flutter才是真正的在Native App中React思想的实现，实现了曾经我对RN的一些期待(比如高频率交互动画）
- 响应式UI，数据绑定到UI，数据改变后“刷新”UI，不需要获取UI某个元素，手动去更新UI。
- FLutter SDK 高度自由灵活，上层有丰富，开发者也可以从各层次入手
- 开发工具 Android Studio, IntelliJ, or VS Code都提供了Flutter的开发插件，且完善度很高，自动提示用起来也非常爽。支持断点调试，堆栈信息查看 。
- 提供了一套与系统SDK通信的机制 （channel)
- 文档，官方文档 API文档 也是非常完善，也为其他开发者（android ios web reactnative Xamarin) 准备了详细的文档，可以对照学习，UI的思想都差不多。帮助其他的开发者快速入坑Flutter，文档完善度方面这点要比RN强不少了。还有中文翻译文档。以及源码里面的注释即文档。
- 社区 目前有很多学习资料可供参考，谷歌官方也有一些视频。后面会附上链接

### 不足
开黑模式

- 截止目前（2019年 1月初） 。FLutter在 github上issue 处于open状态的多的吓人。处于Open状态的4K+ ，closed 为1.1W。相比RN的数据则为600+ 和1.4W。当然也从另一个方面反映了Flutter受关注度很大，毕竟也是从18年开始火起来的，官方还没来得及解决。这个数据半年后再看
- 在开发过程中遇到两个问题在模式下工作正常 在release模式下，无法work 并在release模式下调试体验不好，这点也挺可怕的。debug 与release模式下运行效果不一样。并且一旦发生了调试起来也很急手 比如我最近遇到的这个问题 [23339](https://github.com/flutter/flutter/issues/23339)
- 虽然号称高性能，但目前综合一些评测评测以及个人体验来看 距离Native 还是有些差距。
- Webview，Flutter本身不提供webview组件， 有webview的插件来提供系统SDK的webview，可以使用Dart对webview进行基本的操作，但是并没有提供Dart 和 JavaScript 通信机制以及对Webview发出请求的拦截，当然开发者完全可以自己去定义，webview 跟系统SDK通信，然后系统SDK使用channel机制与Dart通信 自己曲线实现。
- 图片缓存，Flutter的Image组件本身不支持离线缓存（支持运行时最大1000张图片以及上线100M运行时缓存），比如浏览过的图片，断网重启APP查看，无法加载了，可以结合第三方插件cached_network_image。
- 有时候Hotreload不生效，需要重启运行方可。




小结 ：FLutter 虽有不少问题，但谷歌支持力度很大，对于有React经验的同学 上手也是非常容易的。相比之下RN的最大优势是动态化：苹果开发者协议允许动态下发JavaScript代码。
很难说Flutter是否是未来的趋势，但是我相信，简单化一定是未来的趋势。
另外Hummingbird（FLutter web版代号）release之后，可发挥的空间更大，比如写个编辑器用Hummingbird在web端预览，然后拖拉拽一把梭生成native的主流程。







