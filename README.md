# tabbarReplace
微信小程序tabbar替换

将小程序的最中间的图标变大放到中间。

使用方法：
第一步：将tabbarComponent复制到项目中，然后可以将图标和地址换掉

第二步：到app.json中配置tabBar，将tabbar上的信息填好，tabbar中间的按钮是跳到二级页面，所以不配置在tabBar的list中

第三步：在app.js的onLaunch方法中调用wx.hideTabBar();隐藏系统自带的tabBar

第四步：在app.js中的globalData中加入自定义tabbar的参数，再加入一个方法给tabBar.list配置中的页面使用，代码如下

```
editTabbar: function() {
    let tabbar = this.globalData.tabBar;
    let currentPages = getCurrentPages();
    let _this = currentPages[currentPages.length - 1];
    let pagePath = _this.route;
    (pagePath.indexOf('/') != 0) && (pagePath = '/' + pagePath);
    for (let i in tabbar.list) {
      tabbar.list[i].selected = false;
      (tabbar.list[i].pagePath == pagePath) && (tabbar.list[i].selected = true);
    }
    _this.setData({
      tabbar: tabbar
    });
  },
globalData: {
    userInfo: null,
    tabBar: {
      "backgroundColor": "#ffffff",
      "color": "#979795",
      "selectedColor": "#1c1c1b",
      "list": [
        {
          "pagePath": "/page/index/index",
          "iconPath": "icon/icon_home.png",
          "selectedIconPath": "icon/icon_home_HL.png",
          "text": "首页"
        },
        {
          "pagePath": "/page/myRelease/index",
          "iconPath": "icon/icon_release.png",
          "isSpecial": true,
          "text": "发布"
        },
        {
          "pagePath": "/page/mine/index",
          "iconPath": "icon/icon_mine.png",
          "selectedIconPath": "icon/icon_mine_HL.png",
          "text": "我的"
        }
      ]
     }
  }
```
第五步：在tabBar的list中配置的页面的.js文件的data中加入tabbar:{}，并在onload方法中调用app.editTabbar();

第六步：在tabBar的list中配置的页面的.json文件中加入 "usingComponents": { "tabbar": "../../tabbarComponent/tabbar" } 在.wxml文件中加入 <tabbar tabbar="{{tabbar}}"></tabbar>
