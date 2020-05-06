# Mini商城项目

仅用于测试、微信小程序

## 接⼝⽂档

[项目后台接口文档](https://www.showdoc.cc/128719739414963)

## 帮助⽂档

1. [⼩程序开发⽂档](https://developers.weixin.qq.com/miniprogram/dev/framework/)
2. [mdn](https://developer.mozilla.org/zh-CN/)
3. [阿⾥巴巴字体iconfont](https://www.iconfont.cn/)

# 环境准备

## 注册账号

建议使用全新的邮箱，没有注册过其他小程序或者公众号的。
访问[注册⻚⾯](https://mp.weixin.qq.com/wxopen/waregister?action=step1)，耐⼼完成注册即可。

## 获取APPID

由于后期调⽤微信⼩程序的接⼝等功能，需要索取开发者的⼩程序中的 D APPID ，所以在注册成功后，
可登录，然后获取APPID。
[登录](https://mp.weixin.qq.com/)，成功后可看到如下界⾯

![image-20200506102744939](upload\image-20200506102744939.png)

![image-20200506102845386](upload\image-20200506102845386.png)

## 开发⼯具

[下载地址](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

![image-20200506102943585](upload\image-20200506102943585.png)

微信⼩程序⾃带开发者⼯具，集**开发 ** **预览** **调试** **发布** 于⼀⾝的完整环境。
但是由于编码的体验不算好，因此建议使⽤ `vs code`  + **微信小程序编辑工具** 来实现编码
 `vs code` 负责敲代码， **微信编辑工具** 负责预览

# 项⽬搭建

## 打开微信开发者⼯具

**注意**第⼀次登录的时候需要扫码登录

![image-20200506103723824](upload\image-20200506103723824.png)

## 新建⼩程序项⽬

![image-20200506103904424](upload\image-20200506103904424.png)

## 填写项⽬信息

![image-20200506103942936](upload\image-20200506103942936.png)

## 搭建⽬录结构

| ⽬录名     | 作⽤             |
| ---------- | ---------------- |
| styles     | 存放公共样式     |
| components | 存放组件         |
| lib        | 存放第三⽅库     |
| utils      | ⾃⼰的帮助库     |
| request    | ⾃⼰的接⼝帮助库 |

## 搭建项⽬的⻚⾯

| ⻚⾯名称     | 名称         |
| ------------ | ------------ |
| ⾸⻚         | index        |
| 分类⻚⾯     | category     |
| 商品列表⻚⾯ | goods_list   |
| 商品详情⻚⾯ | goods_detail |
| 购物⻋⻚⾯   | cart         |
| 收藏⻚⾯     | collect      |
| 订单⻚⾯     | order        |
| 搜索⻚⾯     | search       |
| 个⼈中⼼⻚⾯ | user         |
| 意⻅反馈⻚⾯ | feedback     |
| 登录⻚⾯     | login        |
| 授权⻚⾯     | auth         |
| 结算⻚⾯     | pay          |

## 引⼊字体图标

1. 打开[阿⾥巴巴字体图标⽹站](https://www.iconfont.cn/)
2. 选择的图标
3. 添加⾄项⽬
4. 下载到本地
5. 将样式⽂件由`css`修改为`wxss`
6. ⼩程序中引⼊

![image-20200506105106682](upload\image-20200506105106682.png)

```
@import "./styles/iconfont.wxss";
```

## 搭建项⽬tabbar结构

```
"tabBar": {
    "color": "#999",
    "selectedColor": "#ff2d4a",
    "backgroundColor": "#fafafa",
    "position": "bottom",
    "borderStyle": "black",
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页",
        "iconPath": "icons/home.png",
        "selectedIconPath": "icons/home-o.png"
      },
      {
        "pagePath": "pages/category/index",
        "text": "分类",
        "iconPath": "icons/category.png",
        "selectedIconPath": "icons/category-o.png"
      },
      {
        "pagePath": "pages/cart/index",
        "text": "购物车",
        "iconPath": "icons/cart.png",
        "selectedIconPath": "icons/cart-o.png"
      },
      {
        "pagePath": "pages/user/index",
        "text": "我的",
        "iconPath": "icons/my.png",
        "selectedIconPath": "icons/my-o.png"
      }
    ]
  },
```

# ⾸⻚

## 效果

![image-20200506105417678](upload\image-20200506105417678.png)

## 业务逻辑

1. 使⽤`tabbar`实现底部导航功能
2. 使⽤⾃定义组件的⽅式实现**头部搜索框**
3. 加载**轮播图**数据
4. 加载**导航**数据
5. 加载**楼层**数据

## 接⼝

1. 获取⾸⻚轮播图数据
2. 获取⾸⻚分类菜单数据
3. 获取⾸⻚楼层数据

## 关键技术

1. ⼩程序内置`request API`

   ```
   // 发起网络请求
   wx.request({
       url: "",
       data: {},
       header: { "content-type": "application/json" },
       method: "GET",
       dataType: "json",
       responseType: "text",
       success: (result) => {},
       fail: () => {},
       complete: () => {},
   });
   ```

2. `es6`的 `promise`

   ```
   return new Promise((resolve, reject) => {
     wx.request({
         ...params,
         success: (result) => {
          resolve(result.data.message);
         },
         fail: (err) => {
          reject(err);
         },
     });
   });
   ```

   ```
     // 获取分类导航数据
     getCateList() {
       request({
         url: "/home/catitems",
       }).then((result) => {
         this.setData({
           catesList: result,
         });
       });
     },
   ```

3. ⼩程序`swiper`组件

   ```
   <swiper autoplay indicator-dots circular>
       <swiper-item wx:for="{{swiperList}}" wx:key="goods_id">
           <navigator url="{{item.navigator_url_index}}">
           	<image mode="widthFix" src="{{item.image_src}}" />
           </navigator>
       </swiper-item>
   </swiper>
   ```

4. ⾃定义组件实现搜索框

# 分类⻚⾯

## 效果

![image-20200506110526804](upload\image-20200506110526804.png)

## 业务逻辑

1. 加载分类⻚⾯数据
2. 点击左侧菜单，右侧数据动态渲染

## 接⼝

1. 分类⻚⾯数据

## 关键技术

1. `scroll-view`组件

   ```
   <!-- 左侧菜单 -->
   <scroll-view class="left_menu" scroll-y="{{true}}">
       <view class="menu_item {{index===currentIndex?'active':''}}" 
       	wx:for="{{leftMenuList}}" 
       	wx:key="*this" 
       	bindtap="handleItemTap" 
       	data-index="{{index}}">
       		{{item}}
       </view>
   </scroll-view>
   ```

2. web中的本地存储和 小程序中的本地存储的区别

   - 写代码的方式不一样了 

      web:

     ​	`localStorage.setItem("key","value")`

     ​	`localStorage.getItem("key")`

     小程序中:

     ​	`wx.setStorageSync("key", "value");` 

     ​	`wx.getStorageSync("key");`

   - 存的时候 有没有做类型转换

     web: 不管存入的是什么类型的数据，最终都会先调用以下 toString(),把数据变成了字符串 再存入进去

     小程序: 不存在 类型转换的这个操作 存什么类似的数据进去，获取的时候就是什么类型

3. `es7`的 `async` 和 `await`

   ```
     // 获取分类数据
     async getCates() {
       // 1 使用es7的async await来发送请求
       const res = await request({ url: "/categories" });
       this.Cates = res;
       // 把接口的数据存入到本地存储中
       wx.setStorageSync("cates", { time: Date.now(), data: this.Cates });
       // 构造左侧的大菜单数据
       let leftMenuList = this.Cates.map((v) => v.cat_name);
       // 构造右侧的商品数据
       let rightContent = this.Cates[0].children;
       this.setData({
         leftMenuList,
         rightContent,
       });
     },
   ```

4. ⼩程序中⽀持es7的async语法

   es7的  `async` 号称是解决回调的最终⽅案

   - 在⼩程序的开发⼯具中，勾选es6转es5语法

   - 下载facebook的regenerator库中的[regenerator/packages/regenerator-runtime/runtime.js](regenerator/packages/regenerator-runtime/runtime.js)

   - 在⼩程序⽬录下新建⽂件夹 `lib/runtime/runtime.js` ，将代码拷⻉进去

   - 在每⼀个需要使⽤async语法的⻚⾯js⽂件中，都引⼊（不能全局引⼊）

     ```
     import regeneratorRuntime from '../../lib/runtime/runtime';
     ```

# 商品列表⻚⾯

## 效果

![image-20200506115054804](upload\image-20200506115054804.png)

## 业务逻辑 

1. 加载商品列表数据
2. 启⽤下拉⻚⾯功能
   - ⻚⾯的json⽂件中开启设置`enablePullDownRefresh:true`
   - ⻚⾯的js中，绑定事件`onPullDownRefresh`
3. 启⽤上拉⻚⾯功能 `onReachBottom` ⻚⾯触底事件
4. 加载下⼀⻚功能

## 接⼝

1. 商品列表搜索

## 关键技术

1. ⼩程序配置⽂件中启⽤上拉和下拉功能

   ```
   /* 
   用户上滑页面 滚动条触底 开始加载下一页数据
     1 找到滚动条触底事件  onReachBottom
     2 判断还有没有下一页数据
       1 获取到总页数  只有总条数
         总页数 = Math.ceil(总条数 /  页容量  pagesize)
         总页数     = Math.ceil( 23 / 10 ) = 3
       2 获取到当前的页码  pagenum
       3 判断一下 当前的页码是否大于等于 总页数 
         表示 没有下一页数据
     3 假如没有下一页数据 弹出一个提示
     4 假如还有下一页数据 来加载下一页数据
       1 当前的页码 ++
       2 重新发送请求
       3 数据请求回来  要对data中的数组 进行 拼接 而不是全部替换！！！
   */
   // 页面上滑 滚动条触底事件
   onReachBottom() {
       //  1 判断还有没有下一页数据
       if (this.QueryParams.pagenum >= this.totalPages) {
           // 没有下一页数据
           wx.showToast({ title: "没有下一页数据" });
       } else {
           // 还有下一页数据
           this.QueryParams.pagenum++;
           this.getGoodsList();
       }
   },
   // 获取商品列表数据
   async getGoodsList() {
       const res = await request({ url: "/goods/search", data: this.QueryParams });
       // 获取 总条数
       const total = res.total;
       // 计算总页数
       this.totalPages = Math.ceil(total / this.QueryParams.pagesize);
       this.setData({
           // 拼接了数组
           goodsList: [...this.data.goodsList, ...res.goods],
       });
       // 关闭下拉刷新的窗口 如果没有调用下拉刷新的窗口 直接关闭也不会报错
       wx.stopPullDownRefresh();
   },
   ```

   ```
   /* 下拉刷新页面
     1 触发下拉刷新事件 需要在页面的json文件中开启一个配置项
       找到 触发下拉刷新的事件
     2 重置 数据 数组 
     3 重置页码 设置为1
     4 重新发送请求
     5 数据请求回来 需要手动的关闭 等待效果
   */
   "enablePullDownRefresh": true,
   "backgroundTextStyle": "dark"
   
   // 下拉刷新事件
   onPullDownRefresh() {
       // 1 重置数组
       this.setData({
       	goodsList: [],
       });
       // 2 重置页码
       this.QueryParams.pagenum = 1;
       // 3 发送请求
       this.getGoodsList();
   },
   ```

2. 搜索框和tab栏是⼩程序的⾃定义组件(有组件事件和参数交互)

# 商品详情⻚⾯

## 效果

![image-20200506120848388](upload\image-20200506120848388.png)

## 业务逻辑

1. 渲染商品详情数据
2. 点击图⽚，[调出图⽚画廊，进⾏预览](https://developers.weixin.qq.com/miniprogram/dev/api/wx.previewImage.html)
3. 点击收藏
4. 联系客服
5. 分享功能
6. 加⼊购物⻋

## 接⼝

1. 获取详情数据接⼝
2. 加⼊购物⻋接⼝使⽤本地存储来维护购物⻋数据
3. ⽴即购买接⼝(相当于是创建订单接⼝)

## 关键技术

1. swiper组件

   ```
   <swiper autoplay circular indicator-dots>
       <swiper-item 
       	wx:for="{{goodsObj.pics}}" 
       	wx:key="pics_id" 
       	bindtap="handlePrevewImage" 
       	data-url="{{item.pics_mid}}">
       		<image mode="widthFix" src="{{item.pics_mid}}"></image>
       </swiper-item>
   </swiper>
   ```

2. 预览大图

   ```
   /*点击轮播图 预览大图
     1 给轮播图绑定点击事件
     2 调用小程序的api  previewImage
   */
   // 点击轮播图 放大预览
   handlePrevewImage(e) {
       // 1 先构造要预览的图片数组
       const urls = this.GoodsInfo.pics.map((v) => v.pics_mid);
       // 2 接收传递过来的图片url
       const current = e.currentTarget.dataset.url;
       wx.previewImage({
           current,
           urls,
       });
   },
   ```

3. 本地存储实现收藏功能

   ```
   /* 点击 加入购物车
     1 先绑定点击事件
     2 获取缓存中的购物车数据 数组格式 
     3 先判断 当前的商品是否已经存在于 购物车
     4 已经存在 修改商品数据  执行购物车数量++ 重新把购物车数组 填充回缓存中
     5 不存在于购物车的数组中 直接给购物车数组添加一个新元素 新元素 带上 购买数量属性 num  重新把购物车数组 填充回缓存中
     6 弹出提示
     
     商品收藏
     1 页面onShow的时候  加载缓存中的商品收藏的数据
     2 判断当前商品是不是被收藏 
       1 是 改变页面的图标
       2 不是 。。
     3 点击商品收藏按钮 
       1 判断该商品是否存在于缓存数组中
       2 已经存在 把该商品删除
       3 没有存在 把商品添加到收藏数组中 存入到缓存中即可
   */
   // 点击 加入购物车
   handleCartAdd() {
       // 1 获取缓存中的购物车 数组
       let cart = wx.getStorageSync("cart") || [];
       // 2 判断 商品对象是否存在于购物车数组中
       let index = cart.findIndex((v) => v.goods_id === this.GoodsInfo.goods_id);
       if (index === -1) {
           //3  不存在 第一次添加
           this.GoodsInfo.num = 1;
           this.GoodsInfo.checked = true;
           cart.push(this.GoodsInfo);
       } else {
           // 4 已经存在购物车数据 执行 num++
           cart[index].num++;
       }
       // 5 把购物车重新添加回缓存中
       wx.setStorageSync("cart", cart);
       // 6 弹窗提示
       wx.showToast({
           title: "加入成功",
           icon: "success",
           // true 防止用户 手抖 疯狂点击按钮
           mask: true,
       });
   },
   // 点击 商品收藏图标
   handleCollect() {
       let isCollect = false;
       // 1 获取缓存中的商品收藏数组
       let collect = wx.getStorageSync("collect") || [];
       // 2 判断该商品是否被收藏过
       let index = collect.findIndex(
       	(v) => v.goods_id === this.GoodsInfo.goods_id
       );
       // 3 当index！=-1表示 已经收藏过
       if (index !== -1) {
           // 能找到 已经收藏过了  在数组中删除该商品
           collect.splice(index, 1);
           isCollect = false;
           wx.showToast({
               title: "取消成功",
               icon: "success",
               mask: true,
           });
       } else {
           // 没有收藏过
           collect.push(this.GoodsInfo);
           isCollect = true;
           wx.showToast({
               title: "收藏成功",
               icon: "success",
               mask: true,
           });
       }
       // 4 把数组存入到缓存中
       wx.setStorageSync("collect", collect);
       // 5 修改data中的属性  isCollect
       this.setData({
       	isCollect,
       });
   },
   ```

4. 联系客服⼩程序管理后台中直接添加即可

   ```
   <view class="tool_item">
       <view class="iconfont icon-kefu"></view>
       <view>客服</view>
       <button open-type="contact"></button>
   </view>
   ```

5. 富⽂本标签渲染富⽂本

   ```
   // iphone部分手机 不识别 webp图片格式
   // 最好找到后台 让他进行修改
   // 临时自己改 确保后台存在 1.webp => 1.jpg
   goods_introduce: goodsObj.goods_introduce.replace(/\.webp/g, ".jpg"),
   
   <view class="goods_info_content">
       <!-- 富文本 -->
   	<rich-text nodes="{{goodsObj.goods_introduce}}"></rich-text>
   </view>
   ```

# 收藏⻚

## 效果

![image-20200506124114207](upload\image-20200506124114207.png)

## 业务逻辑

1. 获取本地存储中的数据进⾏渲染
2. 点击商品可以跳转到商品详情⻚⾯

## 接⼝ 

⽆

## 关键技术

1. ⼩程序⾃定义组件
2. 本地存储加载收藏数据

# 购物⻋⻚⾯

## 效果

![image-20200506124342327](upload\image-20200506124342327.png)

## 业务逻辑

1. 渲染购物⻋数据
2. 添加收货地址
3. 修改商品数量
4. 单选和全选功能

## 接⼝

1. 获取购物⻋数据本地存储实现
2. [调⽤微信的收货地址](https://developers.weixin.qq.com/miniprogram/dev/api/wx.chooseAddress.html)

## 关键技术

1. ⼩程序选择收货地址 api

   ```
   /* 
     获取用户的收货地址
     1 绑定点击事件
     2 调用小程序内置 api  获取用户的收货地址  wx.chooseAddress
     3 获取用户对小程序所授予获取地址的权限状态 scope
       1 假设用户点击获取收货地址的提示框 确定  authSetting scope.address 
         scope 值 true 直接调用获取收货地址
       2 假设用户从来没有调用过收货地址的api 
         scope undefined 直接调用获取收货地址
       3 假设用户点击获取收货地址的提示框 取消   
         scope 值 false 
         1 诱导用户自己打开授权设置页面(wx.openSetting) 当用户重新给与 获取地址权限的时候 
         2 获取收货地址
       4 把获取到的收货地址存入到本地存储中 
   */
   /**
    * promise 形式  getSetting
    */
   export const getSetting = () => {
     return new Promise((resolve, reject) => {
       wx.getSetting({
         success: (result) => {
           resolve(result);
         },
         fail: (err) => {
           reject(err);
         },
       });
     });
   };
   /**
    * promise 形式  chooseAddress
    */
   export const chooseAddress = () => {
     return new Promise((resolve, reject) => {
       wx.chooseAddress({
         success: (result) => {
           resolve(result);
         },
         fail: (err) => {
           reject(err);
         },
       });
     });
   };
   /**
    * promise 形式  openSetting
    */
   export const openSetting = () => {
     return new Promise((resolve, reject) => {
       wx.openSetting({
         success: (result) => {
           resolve(result);
         },
         fail: (err) => {
           reject(err);
         },
       });
     });
   };
   
   // 点击 收货地址
   async handleChooseAddress() {
       try {
           // 1 获取 权限状态
           const res1 = await getSetting();
           const scopeAddress = res1.authSetting["scope.address"];
           // 2 判断 权限状态
           if (scopeAddress === false) {
           	await openSetting();
           }
           // 4 调用获取收货地址的 api
           let address = await chooseAddress();
           address.all =
               address.provinceName +
               address.cityName +
               address.countyName +
               address.detailInfo;
           // 5 存入到缓存中
           wx.setStorageSync("address", address);
       } catch (error) {
       	console.log(error);
       }
   },
   ```

2. ⼩程序复选框组件

```
/*
	总价格和总数量
  1 都需要商品被选中 我们才拿它来计算
  2 获取购物车数组
  3 遍历
  4 判断商品是否被选中
  5 总价格 += 商品的单价 * 商品的数量
  5 总数量 +=商品的数量
  6 把计算后的价格和数量 设置回data中即可
*/
// 设置购物车状态同时 重新计算 底部工具栏的数据 全选 总价格 购买的数量
setCart(cart) {
    let allChecked = true;
    // 1 总价格 总数量
    let totalPrice = 0;
    let totalNum = 0;
    cart.forEach((v) => {
        if (v.checked) {
            totalPrice += v.num * v.goods_price;
            totalNum += v.num;
        } else {
        	allChecked = false;
        }
    });
    // 判断数组是否为空
    allChecked = cart.length != 0 ? allChecked : false;
    this.setData({
        cart,
        totalPrice,
        totalNum,
        allChecked,
    });
    wx.setStorageSync("cart", cart);
},
/*
	商品的选中
  1 绑定change事件
  2 获取到被修改的商品对象
  3 商品对象的选中状态 取反
  4 重新填充回data中和缓存中
  5 重新计算全选。总价格 总数量。。。
*/
// 商品的选中
handeItemChange(e) {
    // 1 获取被修改的商品的id
    const goods_id = e.currentTarget.dataset.id;
    // 2 获取购物车数组
    let { cart } = this.data;
    // 3 找到被修改的商品对象
    let index = cart.findIndex((v) => v.goods_id === goods_id);
    // 4 选中状态取反
    cart[index].checked = !cart[index].checked;
    this.setCart(cart);
},
/*
	全选和反选
  1 全选复选框绑定事件 change
  2 获取 data中的全选变量 allChecked
  3 直接取反 allChecked=!allChecked
  4 遍历购物车数组 让里面 商品 选中状态跟随  allChecked 改变而改变
  5 把购物车数组 和 allChecked 重新设置回data 把购物车重新设置回 缓存中
*/
// 商品全选功能
handleItemAllCheck() {
    // 1 获取data中的数据
    let { cart, allChecked } = this.data;
    // 2 修改值
    allChecked = !allChecked;
    // 3 循环修改cart数组 中的商品选中状态
    cart.forEach((v) => (v.checked = allChecked));
    // 4 把修改后的值 填充回data或者缓存中
    this.setCart(cart);
},
/*
	商品数量的编辑
  1 "+" "-" 按钮 绑定同一个点击事件 区分的关键 自定义属性 
    1 “+” "+1"
    2 "-" "-1"
  2 传递被点击的商品id goods_id
  3 获取data中的购物车数组 来获取需要被修改的商品对象
  4 当 购物车的数量 =1 同时 用户 点击 "-"
    弹窗提示(showModal) 询问用户 是否要删除
    1 确定 直接执行删除
    2 取消  什么都不做 
  4 直接修改商品对象的数量 num
  5 把cart数组 重新设置回 缓存中 和data中 this.setCart
*/
// 商品数量的编辑功能
async handleItemNumEdit(e) {
    // 1 获取传递过来的参数
    const { operation, id } = e.currentTarget.dataset;
    // 2 获取购物车数组
    let { cart } = this.data;
    // 3 找到需要修改的商品的索引
    const index = cart.findIndex((v) => v.goods_id === id);
    // 4 判断是否要执行删除
    if (cart[index].num === 1 && operation === -1) {
    // 4.1 弹窗提示
    const res = await showModal({ content: "您是否要删除？" });
    	if (res.confirm) {
            cart.splice(index, 1);
            this.setCart(cart);
        }
    } else {
        // 4  进行修改数量
        cart[index].num += operation;
        // 5 设置回缓存和data中
        this.setCart(cart);
    }
},

/**
 *  promise 形式  showModal
 * @param {object} param0 参数
 */
export const showModal = ({ content }) => {
  return new Promise((resolve, reject) => {
    wx.showModal({
      title: "提示",
      content: content,
      success: (res) => {
        resolve(res);
      },
      fail: (err) => {
        reject(err);
      },
    });
  });
};
```

# ⽀付⻚⾯

## 效果

![image-20200506130355039](upload\image-20200506130355039.png)

## 业务逻辑

1. 获取微信收货地址
2. 渲染购物⻋中要结算的商品
3. 实现⽀付
   - 获取微信的登录信息
   - 获取⾃⼰后台返回的⽀付相关参数
   - 调⽤微信接⼝实现⽀付
   - ⽀付成功创建订单
   - 跳转到订单⻚⾯

## ⽀付流程

![image-20200506132340262](upload\image-20200506132340262.png)

## 接⼝

1. 获取预⽀付参数
2. 创建订单
3. 更新订单状态

## 关键技术

1. ⼩程序⽀付api

```
/*
微信支付
  1 哪些人 哪些帐号 可以实现微信支付
    1 企业帐号 
    2 企业帐号的小程序后台中 必须 给开发者 添加上白名单 
      1 一个 appid 可以同时绑定多个开发者
      2 这些开发者就可以公用这个appid 和 它的开发权限
支付按钮
  1 先判断缓存中有没有token
  2 没有 跳转到授权页面 进行获取token 
  3 有token 。。。
  4 创建订单 获取订单编号
  5 已经完成了微信支付
  6 手动删除缓存中 已经被选中了的商品 
  7 删除后的购物车数据 填充回缓存
  8 再跳转页面   
*/
// 点击 支付
async handleOrderPay() {
    try {
        // 1判断缓存中有没有token
        const token = wx.getStorageSync("token");
        // 2 判断
        if (!token) {
            wx.navigateTo({
            	url: "/pages/auth/index",
            });
            return;
        }
        // 3 创建订单
        // 3.1 准备 请求头参数
        // const header = { Authorization: token };
        // 3.2 准备 请求体参数
        const order_price = this.data.totalPrice;
        const consignee_addr = this.data.address.all;
        const cart = this.data.cart;
        let goods = [];
        cart.forEach((v) =>
            goods.push({
                goods_id: v.goods_id,
                goods_number: v.num,
                goods_price: v.goods_price,
            })
        );
        const orderParams = { order_price, consignee_addr, goods };
        // 4 准备发送请求 创建订单 获取订单编号
        const { order_number } = await request({
            url: "/my/orders/create",
            method: "POST",
            data: orderParams,
        });
        // 5 发起 预支付接口
        const { pay } = await request({
            url: "/my/orders/req_unifiedorder",
            method: "POST",
            data: { order_number },
        });
        // 6 发起微信支付
        await requestPayment(pay);
        // 7 查询后台 订单状态
        const res = await request({
            url: "/my/orders/chkOrder",
            method: "POST",
            data: { order_number },
        });
        await showToast({ title: "支付成功功能待开发！" });
        // 8 手动删除缓存中 已经支付了的商品
        let newCart = wx.getStorageSync("cart");
        newCart = newCart.filter((v) => !v.checked);
        wx.setStorageSync("cart", newCart);
        // 9 支付成功了 跳转到订单页面
        wx.navigateTo({
        	url: "/pages/order/index",
        });
    } catch (error) {
        await showToast({ title: "支付失败" });
        console.log(error);
    }
},

/**
 * promise 形式的 小程序的微信支付
 * @param {object} pay 支付所必要的参数
 */
export const requestPayment = (pay) => {
  return new Promise((resolve, reject) => {
    wx.requestPayment({
      ...pay,
      success: (result) => {
        resolve(result);
      },
      fail: (err) => {
        reject(err);
      },
    });
  });
};
```

# 授权⻚⾯

## 效果

![image-20200506133549428](upload\image-20200506133549428.png)

## 业务逻辑

![image-20200506133600943](upload\image-20200506133600943.png)

1. 获取⽤⼾信息

   返回`encryptedData`,`rawData`,`iv`,`signature`

2. ⼩程序登录

   返回`code`

3. 提交数据到⾃⼰的后台执⾏post请求提交数据

   `encryptedData,rawData,iv,signature code`

4. 将 `token` 和⽤⼾数据 `rawData` 存⼊本地存储

## 接⼝

1. 提交数据到后台返回token

## 关键技术

1. 获取用户信息

   ```
   <button type="primary" plain open-type="getUserInfo" 				bindgetuserinfo="handleGetUserInfo">
       获取授权
   </button>
   
   // 获取用户信息
   async handleGetUserInfo(e) {
       try {
           // 1 获取用户信息
           const { encryptedData, rawData, iv, signature } = e.detail;
           // 2 获取小程序登录成功后的code
           const { code } = await login();
           const loginParams = { encryptedData, rawData, iv, signature, code };
           //  3 发送请求 获取用户的token
           const { token } = await request({
               url: "/users/wxlogin",
               data: loginParams,
               method: "post",
           });
           // 4 把token存入缓存中 同时跳转回上一个页面
           wx.setStorageSync("token", token);
           wx.navigateBack({
           	delta: 1,
           });
       } catch (error) {
       	console.log(error);
       }
   },
   
   
   /**
    * promise 形式  login
    */
   export const login = () => {
     return new Promise((resolve, reject) => {
       wx.login({
         timeout: 10000,
         success: (result) => {
           resolve(result);
         },
         fail: (err) => {
           reject(err);
         },
       });
     });
   };
   ```

# 订单列表⻚⾯

## 效果

![image-20200506134606959](upload\image-20200506134606959.png)

## 业务逻辑

1. 根据不同的的状态去加载不同的订单数据
2. 点击标题紧挨着对应数据

## 接⼝

1. 查询订单数据

## 关键技术

1. ⼩程序⾃定义组件的传参 ⽗向⼦动态传参

   ```
   Component({
     /**
      * 组件的属性列表
      */
     properties: {
       tabs: {
         type: Array,
         value: [],
       },
     },
     /**
      * 组件的方法列表
      */
     methods: {
       // 点击事件
       handleItemTap(e) {
         // 1 获取点击的索引
         const { index } = e.currentTarget.dataset;
         // 2 触发 父组件中的事件 自定义
         this.triggerEvent("tabsItemChange", { index });
       },
     },
   });
   
   <Tabs tabs="{{tabs}}" bindtabsItemChange="handleTabsItemChange">
   	...
   </Tabs>
   
   handleTabsItemChange(e) {
       // 1 获取被点击的标题索引
       const { index } = e.detail;
       this.changeTitleByIndex(index);
       // 2 重新发送请求 type=1 index=0
       this.getOrders(index + 1);
   },
   ```

2. 加载订单

   ```
   /* 
   页面被打开的时候 onShow 
     0 onShow 不同于onLoad 无法在形参上接收 options参数 
     0.5 判断缓存中有没有token 
       1 没有 直接跳转到授权页面
       2 有 直接往下进行 
     1 获取url上的参数type
     2 根据type来决定页面标题的数组元素 哪个被激活选中 
     2 根据type 去发送请求获取订单数据
     3 渲染页面
   点击不同的标题 重新发送请求来获取和渲染数据 
    */
   
   onShow() {
       const token = wx.getStorageSync("token");
       if (!token) {
           wx.navigateTo({
           	url: "/pages/auth/index",
           });
       	return;
       }
       // 1 获取当前的小程序的页面栈-数组 长度最大是10页面
       let pages = getCurrentPages();
       // 2 数组中 索引最大的页面就是当前页面
       let currentPage = pages[pages.length - 1];
       // 3 获取url上的type参数
       const { type } = currentPage.options;
       // 4 激活选中页面标题 当 type=1 index=0
       this.changeTitleByIndex(type - 1);
       this.getOrders(type);
   },
   // 获取订单列表的方法
   async getOrders(type) {
       const res = await request({ url: "/my/orders/all", data: { type } });
       this.setData({
           orders: res.orders.map((v) => ({
               ...v,
               create_time_cn: new Date(v.create_time * 1000).toLocaleString(),
           })),
       });
   },
   // 根据标题索引来激活选中 标题数组
   changeTitleByIndex(index) {
       // 2 修改源数组
       let { tabs } = this.data;
       tabs.forEach((v, i) =>
       	i === index ? (v.isActive = true) : (v.isActive = false)
       );
       // 3 赋值到data中
       this.setData({
       	tabs,
       });
   },
   ```

# 搜索⻚⾯

## 效果

![image-20200506135930426](upload\image-20200506135930426.png)

## 接⼝

1. 搜索建议查询

## 业务逻辑

1. 获取输⼊框的值进⾏搜索和渲染
2. 点击取消按钮时清除输⼊状态，修改⻚⾯模样

## 关键技术

1. ⼩程序输⼊框组件

   ```
   <view class="search_row">
       <input value="{{inpValue}}" placeholder="请输入您要搜索的商品" 	bindinput="handleInput">
       </input>
       <button bindtap="handleCancel" hidden="{{!isFocus}}">取消</button>
   </view>
   ```

2. 输⼊值改变时，为了提⾼性能，使⽤防抖技术

   ```
   /*
   输入框绑定 值改变事件 input事件
     1 获取到输入框的值
     2 合法性判断 
     3 检验通过 把输入框的值 发送到后台
     4 返回的数据打印到页面上
   防抖 （防止抖动） 定时器  节流 
     0 防抖 一般 输入框中 防止重复输入 重复发送请求
     1 节流 一般是用在页面下拉和上拉 
     1 定义全局的定时器id
   */
   
   TimeId: -1,
   // 输入框的值改变 就会触发的事件
   handleInput(e) {
       // 1 获取输入框的值
       const { value } = e.detail;
       // 2 检测合法性
       if (!value.trim()) {
           this.setData({
               goods: [],
               isFocus: false,
       	});
           clearTimeout(this.TimeId);
           // 值不合法
           return;
       }
       // 3 准备发送请求获取数据
       this.setData({
       	isFocus: true,
       });
       clearTimeout(this.TimeId);
       this.TimeId = setTimeout(() => {
           this.qsearch(value);
           }, 
       1000);
    },
   // 发送请求获取搜索建议 数据
   async qsearch(query) {
       const res = await request({ url: "/goods/qsearch", data: { query } });
       this.setData({
       	goods: res,
       });
   },
   // 点击 取消按钮
   handleCancel() {
       this.setData({
           inpValue: "",
           isFocus: false,
           goods: [],
       });
   },
   ```

# 个⼈中⼼⻚⾯

## 效果

![image-20200506141517905](upload\image-20200506141517905.png)

## 业务逻辑

1. 获取登录信息
2. 加载收藏信息
3. 查询订单状态

## 接⼝

1. [获取⽤⼾信息](https://developers.weixin.qq.com/miniprogram/dev/api/wx.getUserInfo.html?search-key=getuser)
2. 获取收藏数据从本地存储中获取
3. 获取订单数据

## 关键技术

1. [css属性filter的使⽤](https://www.runoob.com/cssref/css3-pr-filter.html)

   ```
   .user_bg {
       height: 50vh;
       // 高斯模糊
       filter: blur(10rpx);
   }
   ```

# 意⻅反馈⻚⾯

## 效果

![image-20200506141906353](upload\image-20200506141906353.png)

## 业务逻辑

1. 点击+ 可以选择本地图⽚，并且显⽰到⻚⾯上

2. 点击提交可以上传图⽚到接⼝地址新浪图床上

   ```
   https://images.ac.cn/Home/Index/UploadAction/
   ```

3. 点击图⽚，会移除⾃⼰

4. 点击tab栏的标题，可以切换选中效果

## 接⼝

无

## 关键技术

1. ⾃定义组件tab

   ```
   /*
   点击 “+” 触发tap点击事件
     1 调用小程序内置的 选择图片的 api
     2 获取到 图片的路径  数组
     3 把图片路径 存到 data的变量中
     4 页面就可以根据 图片数组 进行循环显示 自定义组件
   */
   // 点击 “+” 选择图片
   handleChooseImg() {
       // 2 调用小程序内置的选择图片api
       wx.chooseImage({
           // 同时选中的图片的数量
           count: 9,
           // 图片的格式  原图  压缩
           sizeType: ["original", "compressed"],
           // 图片的来源  相册  照相机
           sourceType: ["album", "camera"],
           success: (result) => {
               this.setData({
                   // 图片数组 进行拼接
                   chooseImgs: [...this.data.chooseImgs, ...result.tempFilePaths],
               });
           },
       });
   },
   ```

2. ⾃定义组件图⽚删除组件

   ```
   /*
     点击 自定义图片 组件
     1 获取被点击的元素的索引
     2 获取 data中的图片数组
     3 根据索引 数组中删除对应的元素
     4 把数组重新设置回data中
   */
   // 点击 自定义图片组件
   handleRemoveImg(e) {
       // 2 获取被点击的组件的索引
       const { index } = e.currentTarget.dataset;
       // 3 获取data中的图片数组
       let { chooseImgs } = this.data;
       // 4 删除元素
       chooseImgs.splice(index, 1);
       this.setData({
       	chooseImgs,
       });
   },
   ```

3. ⼩程序上传⽂件api

   ```
   /*
   点击 “提交”
     1 获取文本域的内容 类似 输入框的获取
       1 data中定义变量 表示 输入框内容
       2 文本域 绑定 输入事件 事件触发的时候 把输入框的值 存入到变量中 
     2 对这些内容 合法性验证
     3 验证通过 用户选择的图片 上传到专门的图片的服务器 返回图片外网的链接
       1 遍历图片数组 
       2 挨个上传
       3 自己再维护图片数组 存放 图片上传后的外网的链接
     4 文本域 和 外网的图片的路径 一起提交到服务器 前端的模拟 不会发送请求到后台。。。 
     5 清空当前页面
     6 返回上一页
   */
   // 外网的图片的路径数组
   UpLoadImgs: [],
   // 提交按钮的点击
   handleFormSubmit() {
       // 1 获取文本域的内容 图片数组
       const { textVal, chooseImgs } = this.data;
       // 2 合法性的验证
       if (!textVal.trim()) {
           // 不合法
           wx.showToast({
               title: "输入不合法",
               icon: "none",
               mask: true,
           });
           return;
       }
       // 3 准备上传图片 到专门的图片服务器
       // 上传文件的 api 不支持 多个文件同时上传  遍历数组 挨个上传
       // 显示正在等待的图片
       wx.showLoading({
           title: "正在上传中",
           mask: true,
       });
       // 判断有没有需要上传的图片数组
       if (chooseImgs.length != 0) {
           chooseImgs.forEach((v, i) => {
               wx.uploadFile({
                   // 图片要上传到哪里
                   url: "https://images.ac.cn/api/upload",
                   // 被上传的文件的路径
                   filePath: v,
                   // 上传的文件的名称 后台来获取文件  file
                   name: "file",
                   // 顺带的文本信息
                   formData: {
                   	apiType: "ali",
                   },
                   success: (result) => {
                       let url = JSON.parse(result.data).url;
                       this.UpLoadImgs.push(url);
                       // 所有的图片都上传完毕了才触发
                       if (i === chooseImgs.length - 1) {
                           wx.hideLoading();
                           // console.log("把文本的内容和外网的图片数组 提交到后台中");
                           //  提交都成功了
                           // 重置页面
                           this.setData({
                               textVal: "",
                               chooseImgs: [],
                           });
                           // 返回上一个页面
                           wx.navigateBack({
                           	delta: 1,
                           });
                   	}
                   },
               });
           });
       } else {
           wx.hideLoading();
           console.log("只是提交了文本");
           wx.navigateBack({
           	delta: 1,
           });
       }
   },
   ```