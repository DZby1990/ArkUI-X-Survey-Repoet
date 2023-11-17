# 华为跨平台方案ArkUI-X初探
## ArkUI-X诞生背景
目前来讲，移动端的原生阵营，依然是android和iOS二分天下。华为从前年上线鸿蒙系统之后，长期处于对android的全面兼容模式当中。
2023年8月4号，华为开发者大会正式官宣从2024年开始，鸿蒙系统全面删除AOSP代码，后续系统将不再支持android系统下的所有应用。在此基础上，华为宣布扩展其鸿蒙原生框架ArkUI，命名为ArkUI-X，使其可以同时跨平台移植到android，iOS等移动平台，并在后期逐步支持更多平台。
我们可以从上述新闻中明确：明年开始，华为的新系统将不再支持android，以及以android为载体的跨平台方案，诸如Flutter，ReactNative等。而华为应对这个冲击的方案，就是将其原生框架ArkUI，提升为一个全能的跨平台方案ArkUI-X。
## ArkUI-X的项目前景
ArkUI-X开源项目地址：https://gitee.com/arkui-x
ArkUI-X作为华为鸿蒙OS原生框架ArkUI的扩展，首先它的原生系统鸿蒙OS，在上线两年来，有着比较惊艳的表现，市场评价是比较高的。在市场占有率方面，华为手机目前在国内的2023Q1季度市场份额为9.2%，位列国内第六；移动穿戴设备，更是位列国内第一，在30%上下，应该说经受住了市场考验。
## ArkUI-X对比主流的跨平台方案
### （一）ReactNative，Weex等老一代跨平台方案的劣势
在此之前，市面上已经出现了以ReactNative，weex为代表的跨平台方案。但是就目前来说，这两套方案都没有达到人们的预期，weex已经被阿里放弃。
以RN为代表的时代，为了解决H5时代性能上的巨大差距，Facebook于15年开源了这套UI框架，包括weex在内，他们解决性能问题的法宝，按通俗了讲就是代码映射。以RN为例。
![image](https://github.com/DZby1990/ArkUI-X-Survey-Repoet/assets/12489070/304cf4b5-2838-4d84-b455-9b478f0bf396)

RN本身使用起来，其实和普通的前端框架并无二致，前端开发者的入门成本很低，开发者可以大部分使用JS代码就构建起一个跨平台APP。RN会把应用的JS代码（包括依赖的framework）编译成一个js文件（一般命名为index.android.bundle), RN的整体框架目标就是为了解释运行这个js脚本文件，如果是js扩展的API，则直接通过bridge调用native方法;如果是UI界面，则映射到virtual DOM这个虚拟的JS数据结构中，通过bridge传递到native，然后根据数据属性设置各个对应的真实native的View。bridge是一种JS和JAVA代码通信的机制，用bridge函数传入对方module和method即可得到异步回调的结果。
用一句话概括，就是把前端代码通过virtual DOM生成android或者iOS的原生View控件。所有的RN代码，和原生代码，都是一一对应的关系。听上去很美，RN对比H5确实提高了一些应用的性能，但是经过了几年的发展，也逐渐走进了死胡同。
1. 他的自由度不及原生应用。由于它的实现方式是将RN代码变成原生代码来进行展示，那么必然存在原生可以做而RN无法覆盖到的功能，事实也确实如此，一些复杂的界面效果和动画效果，RN都是无法做到的，而且在功能上也必然滞后于原生平台。
2. 还是性能问题。android的碎片化，导致RN的一些效果或者在较多bridge调用的情况下，确实在中低端机型上表现不佳，毕竟RN归根结底还是需要一层转译，无可避免的会造成性能消耗
3. 最大的问题。当大家以为RN的引入会节约一半开发人员并欣然投入其中之后，却发现RN的使用缺导致了人员的上升。其主要原因，还是在与iOS平台与Android平台的差异性以及Android平台的碎片化。相当大部分大量使用RN进行开发的团队都表示，如果要与原生一样的标准完成需求，那就需要额外的适配工作，并且不可避免的会导致Android和iOS平台使用了不同的代码分支。而且RN底层库的更新，经常会带来两个平台不同的坑以及一些老功能代码的弃用。
### （二）ArkUI-X对比Flutter优势
ArkUI-X作为跨平台方案的后来者，针对目前最火和体验最好的跨平台方案：谷歌出品的Flutter，切实的做到了吸取整体方案的优点，并做了一些针对性的优化。通过这些优化，开发者更容易上手，也更容易结合到已有的项目中。
以下是Flutter的技术框架图和ArkUI-X技术框架图。他们有很多的相同点，可以说是同一个思路的产物，是同一代的产品。他们都基于独立的图形引擎也就是Flutter当中的Render Surface组件，对界面进行渲染。这样做的好处就是使跨平台代码可以像“手机游戏”一样高效的兼容和运行在不同平台。
![image](https://github.com/DZby1990/ArkUI-X-Survey-Repoet/assets/12489070/3efc807d-e22a-4c22-b7c2-3c616f5fceb6)

![image](https://github.com/DZby1990/ArkUI-X-Survey-Repoet/assets/12489070/dd5c2a7a-4d5c-45d1-b8c6-535dc822b7e5)

ArkUI-X对比Flutter，在对应语言写法上更加简洁，上手更加容易
1. 同为声明式UI，Flutter的方案，所有的控件、事件以及属性，都包装成了Widget的子类。假设你在写一个按钮的时候，你每添加一个点击事件、图片、文字、样式等，都需要添加一个相应Widget子类的child。在这个特定的子类当中去设置属性。用过Flutter的应该都对这种特别的无限套娃式写法，印象深刻。这样的缺点就是需要一定的时间适应这种写法，且代码看起来其实并不是很直观。以下是Flutter编写的一个简单按钮例子：
```objc 
child: Padding(
                          padding: new EdgeInsets.only(
                              left: 0.25, right: 0.25, bottom: 0.5),
                          child: GestureDetector(
                              onTapDown: (d) =>
                                  setState(() => this.isDown = true),
                              onTapUp: (d) =>
                                  setState(() => this.isDown = false),
                              onTapCancel: () =>
                                  setState(() => this.isDown = false),
                              onTap: () {
                                _QuickPayStart();
                              },
                              child: Container(
                                  foregroundDecoration: BoxDecoration(
                                    color: isDown
                                        ? Colors.white.withOpacity(0.5)
                                        : Colors.transparent,
                                  ),
                                  color: Colors.white,
                                  child: Column(
                                      children: <Widget>[
                                        Padding(
                                            padding: new EdgeInsets.all(8),
                                            child: Image.asset(
                                              'images/demo_quickpay.png',
                                              fit: BoxFit.cover,
                                              height: 40,
                                              width: 40,
                                            )),
                                        Padding(
                                            padding: new EdgeInsets.only(
                                                left: 5, right: 5, bottom: 5),
                                            child: Text('快捷支付'))
                                      ],
                                      mainAxisAlignment:
                                          MainAxisAlignment.center))))),
```
2. ArkUI-X使用了ArkTS语言，虽然身为静态语言，但是在框架上，很大程度借鉴了Flutter并进行了优化。举几个简单的例子。他们针对多终端适配，在横向和纵向布局上，都采用了Row() 和 Column()命名的方法。在这个基础上，ArkUI-X优化了套娃式的写法，在代码上使用了“建造者模式”。除此以外，ArkUI-X也省略了Flutter对界面以及控件，可变与不可变状态的声明。这些优化，都使得代码显得更加简洁。以下是一段代码例子，短短29行，就完成了一个列表界面的编写：
```objc 
@Entry
  @Component
  struct ListExample1 {
    @State arr: string[] = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15"]
    @State alignListItem: ListItemAlign = ListItemAlign.Start
  
    build() {
      Column() {
        List({ space: 20, initialIndex: 0 }) {
          ForEach(this.arr, (item) => {
            ListItem() {
              Text('' + item)
                .width('100%')
                .height(100)
                .fontSize(16)
                .textAlign(TextAlign.Center)
                .borderRadius(10)
                .backgroundColor(0xFFFFFF)
            }
            .border({ width: 2, color: Color.Green })
          }, item => item)
        }
        .border({ width: 2, color: Color.Red, style: BorderStyle.Dashed })
        .scrollBar(BarState.On) // 滚动条常驻
        .edgeEffect(EdgeEffect.Spring) // 滚动到边缘再拖动回弹效果
  
      }.width('100%').height('100%').backgroundColor(0xDCDCDC).padding(20)
    }
  }
```
## ArkUI-X对比Flutter，在混合开发上进行了大力优化
1. Flutter在Google发布之初，就附上了“建议开发者完全使用Flutter开发一款APP”的字样。就根本原因上，Flutter为了提升性能，对于android和iOS原生，采用了完全独立的页面栈方案，对于android和iOS原生APP，Flutter更像是一个采用游戏引擎的“游戏应用”。假设一种场景：FlutterA界面-》跳转原生B界面-〉跳转FlutterC界面。Flutter需要启动两套Flutter内核，才可以跑起来，这样子会增加额外的资源消耗，会使加载速度变得很慢。针对这个问题，业内大厂，阿里的闲鱼，出了一套第三方框架，专门去解决这个问题，其基本思路是复用FlutterActivity，通过反射替换其实例，达到复用的效果，使其在启动时减少额外的资源消耗。
2. 针对这个问题，ArkUI-X进行了专门的优化。官方文档即支持在原有android和iOS原生应用的基础上，加入ArkUI进行混合开发。ArkUI-X在编译之后，可以直接生成android或者iOS的原生项目代码。甚至可以直接将ArkUI-X的代码打包成原生sdk，供原生APP使用。以下为官方文档链接：
      https://gitee.com/arkui-x/docs/blob/master/zh-cn/application-dev/tutorial/how-to-integrate-arkui-into-android.md
## ArkUI-X的不足
1. 首先ArkUI-X没有经过时间的检验。Flutter经过三年多的迭代，从支持的平台，到开发者群体人数上，已经有了比较大的发展和进步。
2. ArkUI-X在支持的平台上，目前不如Flutter。暂时只支持android，iOS，鸿蒙。
3. 目前开发套件“DevEco Studio4.0”还是一个beta预览版，还没有发布正式版。暂时还不能进行多平台编译的尝试。
## 总结
个人对ArkUI-X持乐观态度。主要有三个方面：
1. 首先华为的整体框架方案，类似Flutter。Flutter从目前看来，从开发者到市场，还是得到了一定的认可。
2. 就是华为系统的整体体验，对比android和iOS确实有很多独到之处。尤其是超级终端这一项功能，至今还没有其他系统可以模仿。
3. 华为整体的技术能力是比较突出的，ArkUI-X对于华为来说，是华山一条道，直接关系到华为移动产品的存亡。而且华为深耕软硬件结合，应该不会像阿里的weex跨平台方案那样，纯kpi产物，无疾而终。
基于以上观点，个人觉得，在一些不复杂APP产品上，可以择机进行尝试。
