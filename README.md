前端注释不要用冷门的特殊符号，可能会导致运行到手机失败且无明显报错
新版的HbuildX2.6.16已经可以不用手动在main.js里注册组件了（已在H5和app验证），在相关的pages用可以直接使用组件标签。
因SDK和android壳的打包arr版本不符导致的apk打开告警问题，可以在uniapp工程的manifest.json文件app-plus层级下配
"compatible" : {
"ignoreVersion" : true //true表示忽略版本检查提示框，HBuilderX1.9.0及以上版本支持  
}
HBuildX2.6.5版本下,input组件的v-model属性在android的双向绑定有问题,输入过快会出现数字死循环抖动现象,需要用@input里settimeout异步再赋值
HBuildX2.6.5版本下，button组件如果要自定义不可点击时的背景色，就不要再用disabled这个属性了，因为在andorid环境下disabled会覆盖你自定义的颜色
比如以下写法不可取
 <button class="submit" :class="{'btn-disabled': account == 0}" @tap="submit" :disabled="account == 0">确认充值</button>
微信支付：
报“签名不对，请检查签名是否与开放平台上填写的一致”？
检查微信开放平台的签名方式和统一下单的签名方式是否一致，需要都用MD5
如果微信支付首次能支付成功，之后死活不进支付的回调，需要重新安装微信
那就是开放平台的签名不对，应该是数字和小写字母的组合而不是冒号隔开的大写字母和数字
取消微信支付不会走微信的回调fail和complete函数，所以在这两个生命周期去修改防抖的参数是不可行的
软键盘顶起底部元素怎么解决？
写死当前页面高度并用flex布局以及justify-content的space-between属性
如何建一个不可控制map组件，只要在map组件嵌套一个同等大小的cover-view
打包App使用了沉浸式开发由于状态栏的颜色太浅导致手机的状态栏时间、wifi、电量等难以辨识时可以用
plus.navigator.setStatusBarStyle('dark'); // dark/light
进行灵活修改，需要注意的是这个函数在HbuildX2.6.5里
需要放在onShow生命周期，并放在setTimeout里延时设置，且该方法开发模式运行到手机无效，必须云打包或者离线打包才能看到效果

示例：
// #ifdef APP-PLUS
// 修改导航栏颜色主题无法兼容低版本的android机。且测试环境无法验证，必须打包。
setTimeout(() => {
plus.navigator.setStatusBarStyle('dark'); // dark/light
}, 200)
// #endif
v-text指令在text标签使用在H5中没问题，但是在app中会存在无法及时更新的问题，所以不推荐使用该指令
uni打包到App，新增一个组件继承一个已有组件时不要使用任何相同的属性，你永远不知道不同平台的兼容性如何。
js的问题，Date.parse()函数识别"2020-03-20 15:00"以及"2020/03/20 15:00"在H5和android上是不一样的。
for循环下子元素每个都要绑定事件的情况，可以在父节点绑定事件，子解点使用data-x的形式冒泡事件到父节点统一处理
data 必须声明为返回一个初始数据对象的函数；否则页面关闭时，数据不会自动销毁，再次打开该页面时，会显示上次数据。
h5的websocket使用uni.closeSocket的code不能随便传
父传子组件的props如果有函数，h5可以实现，但是小程序会丢失this，所以还是中规中矩用事件传递吧
自定义vue组件打包到app上时，资源文件不要放在组件目录而是放在static目录
canvas用drawImage进行画图的时候，函数调用的现后顺序对画图地址的解析有所不同，如果先加载base64图片再查询本地图片则不能用相对位置，如果先用相对位置加载本地图片再加载base64图片则没问题
this.qrcode为base64图片地址
无错
context.drawImage('../../static/member/share_default_cover.jpg', 64, 104, 167, 140)
context.drawImage(this.qrcode, 34, 271, 64, 64)
报错
context.drawImage(this.qrcode, 84, 104, 128, 128)
context.drawImage('../../static/member/share_default_cover.jpg', 34, 280, 64, 53)
dev模式process.env.NODE_ENV 的值为 development，build 模式 process.env.NODE_ENV 的值为 production。
uni的onSocketMessage流程需要跟在connectSocket流程后面,这个与微信是不一样的，微信的onSocketMessage即使写在connectSocket之前也是可行的
HbuilderX 2.2.2 20190816还不支持通过使用动画的top和left等元素使元素产生移动，只能使用translateX等代替，且translate移动和rotate旋转执行顺序不同会影响transformOrigin的表现
聊天室类型的scroll-view没有好用的函数可以实时自动置底，可以绑定变量到scroll-top然后根据聊天室的消息类型对距离顶部的距离进行修改但是一定要包在this.$nextTick()里
有弹出层后黑色透明幕布防滑动@touchmove.stop.prevent=""，如果弹出框需要滑动则滑动父节点添加@touchmove.stop=""
uni的底部菜单栏自带了对iphoneX这类机型的适配，给菜单栏高度增加了横条菜单栏的高度，此时如果想在菜单栏上面再fxied一个底部栏也需要加上横条高度
bottom: calc(50px + env(safe-area-inset-bottom))
在没有uni菜单栏的页面也只需要配置以下即可
position: fixed;
bottom: 0;
padding-bottom: env(safe-area-inset-bottom); // 以白色填充iphonex的空白底
left: 0;
width: 100%;
background-color: white;
标签rich-text和属性v-html有区别，rich-text无法加载富文本视频video标签，uParse第三方组件可以兼容适配图片和视频的长宽
关于富文本编辑器内的样式问题，可以尝试使用深度选择器 /deep/
公众号里打开微信浏览器并 做过微信认证后导向的网页如果经过switchTab则pages.json的第一个配置页无法跳转页面，报页面找不到，getCurrentPages()长度为0
async await可以阻塞等待网络请求，但是canvas的绘制不会被阻塞
uni.showToast方法会导致页面无法点击和左右滑动
img标签不太适用，尤其是src为一个非static目录下资源则无法显示，可以用view标签代替，样式写在css里，image标签多用来加载网络图片资源
此外img标签加载网络图片地址不能带左括号或者右括号或者同时都有，这会导致图片在H5不可见但是浏览器f12是可以看到图片样子，此情况在微信小程序无影响。
以view标签作为背景图，background资源不能为jpg
showModal接口content和title是必选字段，如果没有title影响不大，但是如果没有content，或者content字段为空，则在h5和微信端没问题，但是在android端则无反应
css样式需规范，多用类选择器，少用标签选择器，因为编译打包后的标签各平台存在差异
H5的子组件onLoad和onReady生命周期都不会走，但是小程序会走
获取当前页的路由信息getCurrentPages()[index].route
父元素的透明度如何不影响子元素，不要用opacity，用rgba()
不支持在标签中使用全局js变量，必须放到data中转一下
uni提供上传视频和图片的接口，那么音频怎么办？
v-for循环的时候，H5的遍历是从1开始，微信是从0开始，例如v-for="i of 5"
若仅期望在 App 上开启下拉刷新，则不要配置页面的 enablePullDownRefresh 属性，而是配置 pullToRefresh->support 为 true。
