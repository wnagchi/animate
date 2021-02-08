## 引入

```javascript
const app=getApp()
app.animatePage({
    data: {
        otherHeight:50
    },
    //节流函数 直接写在里边
    throttle:{
        
	}

})
```



otherHeight: 用来计算scrollHeight

scrollHeight的计算方式为：屏幕整体高度 -  自定义头部高度 - otherHeight

其中  this.data中几经集成了：

1. navHeight ：自定义头部整体高度
2. isIPX：是否为苹果X系列
3. isIphone：是否为苹果手机
4. windowHeight：屏幕高度
5. windowWidth：屏幕宽度
6. scale：屏幕的长宽比  （苹果X为2.17，苹果6为1.78）可用来做长短屏适配













## 内置函数域

把函数写在里边就能可以

书写方式没差异



#### throttle

节流函数 

```javascript
app.animatePage({
    data: {
        otherHeight:50
    },
    //节流函数 直接写在里边
    throttle:{
        upDataList(){
		
        }
	}

})
```

















 

## 内置函数

所有内置函数 直接通过  **this.函数名** 即可调用

**内置异步函数全部为Promise调用方式**



### 接入微信api相关

#### getHeight:

同Jquery使用方法

```javascript
this.getHeight('#xxxxxx')
或者
this.getHeight('.xxxxxx')
```

#xxxxx为通过Id查找

.xxxxx为通过class查找

返回格式为Array

主要用于:查询内容高度, 查询当前元素位置


![输入图片说明](https://images.gitee.com/uploads/images/2021/0208/171025_599efae7_2298050.png "1612763133340.png")




#### getNode:

基本和getHeight用法相同

```javascript
this.getHeight('#xxxxxx')
或者
this.getHeight('.xxxxxx')
```

返回格式为Objcet

返回内容主要为CSS相关属性


![输入图片说明](https://images.gitee.com/uploads/images/2021/0208/171044_107c05e0_2298050.png "1612763326550.png")





#### getWxImg:

 通过微信接口读取图片

注意 **该方法只能用于配置download安全域名才能使用**

```javascr
this.getWxImg('https://xxxxxx.png').then(res=>{
                console.log(res)
            })
```

返回格式为Objcet

返回内容为**图片宽高** 和**图片的本地路径**可以在小程序内直接调用

![输入图片说明](https://images.gitee.com/uploads/images/2021/0208/171054_fa17894f_2298050.png "1612763777493.png")




#### $_prePageToData

往前一页data中塞值并且返回之前页

```
this.$_prePageToData('key','value',true)
```

参数1：塞进去的key

参数2：塞进去的value

参数3：是否返回前一页  默认不返回





#### $_toPath

带参数跳转

```javascript
let data={
    productId:1,
    fromPage:'商品详情页'
}
this.$_toPath('/me/me/me',data)
```

















### wxml直接操作方法

可以省略在js中创建函数的步骤 在wxml中直接调用

#### $_onTab

切换操作

```html
<view>
    <view data-parents='selectTab' data-index='1' bindtap='$_onTab'>切换tab1</view>
    <view data-parents='selectTab' data-index='2' bindtap='$_onTab'>切换tab1</view>
    <view data-parents='selectTab' data-index='3' bindtap='$_onTab'>切换tab1</view>
</view>

<view wx:if="{{selectTab==1}}"></view>
<view wx:if="{{selectTab==2}}"></view>
<view wx:if="{{selectTab==3}}"></view>
```

可以在this.data中直接取到   data-parents设置的值





#### $_selectItem

多选操作 与$_onTab操作方式不同



```html
<view 
      data-parents='arr' 
      data-setname='select' 
      data-index='{{index}}' 
      data-radio='true' 
      bindtap='$_selectItem' 
      wx:for="{{arr}}" 
      class="{{item.select==index?'a_class':'b_class'}}"
      >{{item.name}}</view>
```

data-parents：为循环体的名称 例如：循环体 wx:for="{{arr}}"  那么data-parents=’arr‘

data-setname：为添加的标识名  如果未设置 则自动生成为   ’select‘  

data-index：索引

data-radio：是否为单选  默认为多选



#### $_toPage

跳转

```html
<view 
      data-path='/me/me/me' 
      data-to-type='to' 
      bindtap='$_toPage'
      >跳转到个人中心</view>

<view 
      data-path='/me/me/me' 
      data-to-type='lunch' 
      bindtap='$_toPage'
      >关闭当前页并跳转到个人中心</view>
```



#### $_animateStart

动画执行相关操作  

```html
<view 
      bindtap='$_animateStart'
      data-hide-time='300'
      data-show-time='1000'
      data-setname='first_animate'
      >点击逐渐显示</view>
<view wx:if='{{first_animate.if}}' style='opacity:{{first_animate.start?1:0}};transition:.3s'></view>
```

点击时会生成data-setname设置的对象 对象内部为{if:true,start:true}

注意：data-hide-time ，data-show-time和动画详细执行时间无直接关联

例如 data-show-time是指wx：if和执行style动画时之间的时间差；

因为在小程序渲染数执行时为初始化该节点，所以无法触发transition









### canvas相关方法

待续











### 内置工具方法

内置工具方法多为之前有用到 为方便之后随意封装的方法 

#### $_getData：

```javascript
this.$_getData(e)
```

参数e为event

用于获得在wxml元素中设置的data-xxxx值 

```html
<view data-xxxx='阿喀琉斯开始啦'></view>
```

返回值为 data-中设置的值



#### $_cutArray：

截取数组

```javascript
let arr=[1,3,4,5,6,7,8,3,2,1]
this.$_cutArray(arr,4,6)
//[1,3,4,5]
```

参数1：目标数组

参数2：被分割的长度

参数3：选填 为分割触发  比如例子：在数组长度大于等于6的时候会将目标数组截取为长度为4的数组

返回格式为Array



#### $_shinglier:

将多余文字变成省略号

```javascript
let str='哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈'
this.$_shinglier(str,5,'!!!!')
// 哈哈哈哈哈!!!!
```

参数1：目标字符串

参数2：截取长度

参数3：选填 拼接内容

返回格式为string



#### $_strSm:

将字符串分割为数组

```javascript
let str='哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈'
this.$_strSm(str,5)
//[
	'哈哈哈哈哈',
    '哈哈哈哈哈',
    '哈哈哈哈哈',
    '哈哈哈'
]
```









