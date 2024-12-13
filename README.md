
![](https://img2024.cnblogs.com/blog/468667/202412/468667-20241212191930575-1935181459.gif)
 


【引言】


保质期计算应用是一个基于鸿蒙NEXT框架开发的数字和文本统计组件。用户可以输入商品的生产日期和保质期天数，应用会自动计算并展示相关信息，包括保质状态、剩余天数、生产日期和到期日期。


【环境准备】


• 操作系统：Windows 10


• 开发工具：DevEco Studio NEXT Beta1 Build Version: 5\.0\.3\.806


• 目标设备：华为Mate60 Pro


• 开发语言：ArkTS


• 框架：ArkUI


• API版本：API 12


【实现思路】


1 组件定义


在应用中，我们定义了一个名为ExpiryDateCalculator的组件，其中包含了各种状态变量和方法。通过监听输入文本变化和选择日期变化，实现了自动更新相关信息的功能。


2 UI界面构建


通过构建UI界面，我们使用了列布局和行布局来展示各个信息模块。包括标题展示、统计结果展示、示例和清空按钮、选择生产日期等功能。通过设置字体颜色、背景色、阴影效果等，使界面更加美观和易读。


3 交互功能实现


在交互功能方面，我们实现了输入框焦点状态的切换、清空按钮功能、选择日期功能等。用户可以方便地输入信息并查看计算结果，提升了用户体验和操作便捷性。


【完整代码】



[?](https://github.com)

| 123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286 | `@Entry``@Component``struct ExpiryDateCalculator {``// 定义文本颜色的状态变量，初始值为深灰色``@State` `private` `textColor: string =` `"#2e2e2e"``;``// 定义阴影边框颜色的状态变量，初始值为浅灰色``@State` `private` `shadowColor: string =` `"#d5d5d5"``;``// 定义基础内边距的状态变量，初始值为30``@State` `private` `basePadding: number = 30;``// 定义是否已过期的状态变量，初始值为false``@State` `private` `isExpired: boolean =` `false``;``// 定义生产日期的状态变量，初始值为空字符串``@State` `private` `productionDate: string =` `""``;``// 定义到期日期的状态变量，初始值为空字符串``@State` `private` `expiryDate: string =` `""``;``// 定义剩余有效天数的状态变量，初始值为0``@State` `private` `remainingDays: number = 0;``// 定义主题颜色，初始值为橙色``@State` `private` `themeColor: string | Color = Color.Orange;``// 输入框是否获得了焦点的状态变量，初始值为false``@State isInputFocused: boolean =` `false``;``// 定义监听输入文本变化的状态变量，初始值为"9"``@State @Watch(``'inputChanged'``)` `private` `inputText: string =` `"9"``;``// 定义选择的日期状态变量，初始值为当前日期``@State @Watch(``'inputChanged'``)` `private` `selectedDate: Date =` `new` `Date()` `// 组件即将出现时的操作``aboutToAppear(): void {``// 调用输入变化处理方法``this``.inputChanged()``}` `// 获取年月日的方法``getYearMonthDay(date: Date) {``// 获取年份并格式化为4位数``const year: string = date.getFullYear().toString().padStart(4,` `'0'``);``// 获取月份并格式化为2位数``const month: string = (date.getMonth() + 1).toString().padStart(2,` `'0'``);``// 获取日期并格式化为2位数``const day: string = date.getDate().toString().padStart(2,` `'0'``);``// 返回格式化后的日期字符串``return` ``${year}年${month}月${day}日`;``}` `// 输入变化时的处理方法``inputChanged() {``// 打印当前选择的日期``console.info(`selectedDate:${``this``.selectedDate}`);``// 更新生产日期为选择的日期``this``.productionDate =` `this``.getYearMonthDay(``this``.selectedDate);``// 创建到期日期对象``let` `expiryDate: Date =` `new` `Date(``this``.selectedDate);``// 根据输入的天数更新到期日期``expiryDate.setDate(expiryDate.getDate() + Number(``this``.inputText));``// 更新到期日期为格式化后的字符串``this``.expiryDate =` `this``.getYearMonthDay(expiryDate);``// 判断是否已过期``this``.isExpired = expiryDate.getTime() <` `new` `Date().getTime();``// 计算时间差``const timeDifference = expiryDate.getTime() -` `new` `Date().getTime();``// 计算剩余天数``this``.remainingDays = Math.ceil(timeDifference / (1000 * 60 * 60 * 24));``}` `// 构建UI界面的方法``build() {``// 创建一个列布局容器``Column() {` `// 添加标题``Text(``'保质期计算'``)``.fontColor(``this``.textColor)``// 设置字体颜色``.fontSize(18)``// 设置字体大小``.width(``'100%'``)``// 设置宽度为100%``.height(50)``// 设置高度为50``.textAlign(TextAlign.Center)``// 设置文本对齐方式为居中``.backgroundColor(Color.White)``// 设置背景颜色为白色``.shadow({``// 设置阴影效果``radius: 2,` `// 阴影半径``color:` `this``.shadowColor,` `// 阴影颜色``offsetX: 0,` `// X轴偏移量``offsetY: 5` `// Y轴偏移量``});``// 创建可滚动的容器``Scroll() {``// 在可滚动容器内部创建列布局``Column() {``// 添加统计结果展示``Column() {``// 是否过期``Row() {``Text() {``Span(`保质状态：`)` `// 显示文本“保质状态：”``Span(`${``this``.isExpired ?` `'已过期'` `:` `'未过期'``}`)``// 根据状态显示“已过期”或“未过期”``.fontColor(``this``.isExpired ?` `"#e74c3c"` `:` `"#3ace7d"``)` `// 根据状态设置字体颜色``}``.fontColor(``this``.textColor)` `// 设置字体颜色``.fontSize(16)` `// 设置字体大小``.layoutWeight(1);` `// 设置布局权重``}``.constraintSize({ minHeight: 45 })` `// 设置最小高度``.justifyContent(FlexAlign.SpaceBetween)` `// 设置内容对齐方式``.width(``'100%'``);` `// 设置宽度为100%` `Divider();` `// 添加分隔线``// 剩余天数``Row() {``Text() {``Span(`剩余天数：`)` `// 显示文本“剩余天数：”``Span(`${``this``.remainingDays < 0 ? 0 :` `this``.remainingDays} `).fontColor(Color.Orange)` `// 显示剩余天数，负数显示为0``Span(``'天'``)` `// 显示单位“天”``}``.fontColor(``this``.textColor)` `// 设置字体颜色``.fontSize(16)` `// 设置字体大小``.layoutWeight(1);` `// 设置布局权重``}``.constraintSize({ minHeight: 45 })` `// 设置最小高度``.justifyContent(FlexAlign.SpaceBetween)` `// 设置内容对齐方式``.width(``'100%'``);` `// 设置宽度为100%` `Divider();` `// 添加分隔线``// 生产日期``Row() {``Text() {``Span(`生产日期：`)` `// 显示文本“生产日期：”``Span(`${``this``.productionDate} `)` `// 显示生产日期``}``.fontColor(``this``.textColor)` `// 设置字体颜色``.fontSize(16)` `// 设置字体大小``.layoutWeight(1);` `// 设置布局权重``}``.constraintSize({ minHeight: 45 })` `// 设置最小高度``.justifyContent(FlexAlign.SpaceBetween)` `// 设置内容对齐方式``.width(``'100%'``);` `// 设置宽度为100%` `Divider();` `// 添加分隔线``// 到期日期``Row() {``Text() {``Span(`到期日期：`)` `// 显示文本“到期日期：”``Span(`${``this``.expiryDate} `)` `// 显示到期日期``}``.fontColor(``this``.textColor)` `// 设置字体颜色``.fontSize(16)` `// 设置字体大小``.layoutWeight(1);` `// 设置布局权重``}``.constraintSize({ minHeight: 45 })` `// 设置最小高度``.justifyContent(FlexAlign.SpaceBetween)` `// 设置内容对齐方式``.width(``'100%'``);` `// 设置宽度为100%``}``.alignItems(HorizontalAlign.Start)` `// 设置子项对齐方式``.width(``'650lpx'``)` `// 设置宽度为650像素``.padding(`${``this``.basePadding}lpx`)` `// 设置内边距``.margin({ top: `${``this``.basePadding}lpx` })` `// 设置上边距``.borderRadius(10)` `// 设置圆角``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.shadow({``// 设置阴影效果``radius: 10,` `// 阴影半径``color:` `this``.shadowColor,` `// 阴影颜色``offsetX: 0,` `// X轴偏移量``offsetY: 0` `// Y轴偏移量``});` `// 添加示例和清空按钮``Column() {``Row() {``// 添加文本输入区域``Row() {``TextInput({ text: $$``this``.inputText, placeholder: `请输入保质期天数` })``// 创建文本输入框``.type(InputType.Number)``// 设置输入类型为数字``.layoutWeight(1)``// 设置布局权重``.fontSize(16)``// 设置字体大小``.textAlign(TextAlign.JUSTIFY)``// 设置文本对齐方式``.backgroundColor(Color.Transparent)``// 设置背景色为透明``.padding(0)``// 设置内边距为0``.height(``'100%'``)``// 设置高度为100%``.placeholderColor(``this``.isInputFocused ?` `this``.themeColor : Color.Gray)``// 设置占位符颜色``.fontColor(``this``.isInputFocused ?` `this``.themeColor :` `this``.textColor)``// 设置字体颜色``.caretColor(``this``.themeColor)``// 设置光标颜色``.borderRadius(0)``// 设置圆角为0``.onBlur(() =>` `this``.isInputFocused =` `false``)``// 失去焦点时更新状态``.onFocus(() =>` `this``.isInputFocused =` `true``)``// 获得焦点时更新状态``.width(``'100%'``);` `// 设置宽度为100%``}``.padding(`${``this``.basePadding / 2}lpx`)` `// 设置内边距``.backgroundColor(``"#f2f1fd"``)` `// 设置背景色``.layoutWeight(1)` `// 设置布局权重``.borderWidth(1)` `// 设置边框宽度``.borderRadius(10)` `// 设置圆角为10``.borderColor(``this``.isInputFocused ?` `this``.themeColor : Color.Gray)` `// 根据焦点状态设置边框颜色``.margin({ right: `${``this``.basePadding / 2}lpx` });` `// 设置右边距` `Blank();` `// 添加空白占位符` `// 清空按钮``Text(``'清空'``)``// 显示文本“清空”``.fontColor(``"#e48742"``)``// 设置字体颜色``.fontSize(16)``// 设置字体大小``.padding(`${``this``.basePadding / 2}lpx`)``// 设置内边距``.clickEffect({ level: ClickEffectLevel.LIGHT, scale: 0.8 })``// 点击效果``.backgroundColor(``"#ffefe6"``)``// 设置背景色``.borderRadius(5)``// 设置圆角为5``.onClick(() => {` `// 点击事件处理``this``.inputText =` `""``;` `// 清空输入文本``});``}``.height(45)` `// 设置高度为45``.alignItems(VerticalAlign.Center)` `// 设置内容垂直居中``.justifyContent(FlexAlign.SpaceBetween)` `// 设置内容对齐方式``.width(``'100%'``);` `// 设置宽度为100%` `Divider();` `// 添加分隔线` `// 选择生产日期``Row() {``Text(``'请选择生产日期'``)``// 显示文本“请选择生产日期”``.fontColor(``"#5871ce"``)``// 设置字体颜色``.fontSize(16)``// 设置字体大小``.padding(`${``this``.basePadding / 2}lpx`)``// 设置内边距``.backgroundColor(``"#f2f1fd"``)``// 设置背景色``.borderRadius(5)``// 设置圆角为5``.clickEffect({ level: ClickEffectLevel.LIGHT, scale: 0.8 })` `// 点击效果``Blank();` `// 添加空白占位符``CalendarPicker({ hintRadius: 10, selected:` `this``.selectedDate })``// 创建日历选择器``.edgeAlign(CalendarAlign.END)``// 设置对齐方式``.textStyle({ color:` `"#ff182431"``, font: { size: 20, weight: FontWeight.Normal } })``// 设置文本样式``.margin(10)``// 设置外边距``.onChange((date: Date) => {` `// 日期变化事件处理``this``.selectedDate = date` `// 更新选择的日期``})``}``.height(45)` `// 设置高度为45``.justifyContent(FlexAlign.SpaceBetween)` `// 设置内容对齐方式``.width(``'100%'``);` `// 设置宽度为100%``}``.alignItems(HorizontalAlign.Start)` `// 设置内容水平对齐方式``.width(``'650lpx'``)` `// 设置宽度为650像素``.padding(`${``this``.basePadding}lpx`)` `// 设置内边距``.margin({ top: `${``this``.basePadding}lpx` })` `// 设置上边距``.borderRadius(10)` `// 设置圆角为10``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.shadow({``// 设置阴影效果``radius: 10,` `// 阴影半径``color:` `this``.shadowColor,` `// 阴影颜色``offsetX: 0,` `// X轴偏移量``offsetY: 0` `// Y轴偏移量``});` `// 添加工具介绍``Column() {``Text(``'工具介绍'``)``// 显示文本“工具介绍”``.fontSize(18)``// 设置字体大小为18``.fontWeight(600)``// 设置字体粗细``.fontColor(``this``.textColor);` `// 设置字体颜色为状态变量中定义的文本颜色` `Text(``'输入需要计算保质期商品的生产日期和保质期天数，工具将为您自动计算商品的保质期状态、剩余天数、到期日。'``)``// 显示工具介绍文本``.textAlign(TextAlign.JUSTIFY)``// 设置文本对齐方式为两端对齐``.fontSize(16)``// 设置字体大小为16``.fontColor(``this``.textColor)``// 设置字体颜色为状态变量中定义的文本颜色``.margin({ top: `${``this``.basePadding / 2}lpx` });` `// 设置上边距为基础内边距的一半``}``.alignItems(HorizontalAlign.Start)` `// 设置内容水平对齐方式``.width(``'650lpx'``)` `// 设置宽度为650像素``.padding(`${``this``.basePadding}lpx`)` `// 设置内边距``.margin({ top: `${``this``.basePadding}lpx` })` `// 设置上边距``.borderRadius(10)` `// 设置圆角为10``.backgroundColor(Color.White)` `// 设置背景颜色为白色``.shadow({``// 设置阴影效果``radius: 10,` `// 阴影半径``color:` `this``.shadowColor,` `// 阴影颜色``offsetX: 0,` `// X轴偏移量``offsetY: 0` `// Y轴偏移量``});` `}``}``.scrollBar(BarState.Off)` `// 关闭滚动条``.clip(``false``);` `// 不裁剪内容``}``.height(``'100%'``)` `// 设置高度为100%``.width(``'100%'``)` `// 设置宽度为100%``.backgroundColor(``"#f4f8fb"``);` `// 设置背景颜色为指定颜色``}``}` |
| --- | --- |



　　


 本博客参考[wgetCloud机场](https://longdu.org)。转载请注明出处！
