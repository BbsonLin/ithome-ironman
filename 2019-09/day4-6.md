###### tags: `第 11 屆 iT 邦鐵人賽`

# 用 Flutter 開發一個 Android App 吧 - Day4 ~ Day6


## Day 4

### 主頁面 - 首頁

`page/home.dart`
``` dart
import 'package:flutter/material.dart';

// 主頁面
class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 4,
      child: Scaffold(
        appBar: AppBar(
          titleSpacing: 0.0,
          title: TabBar(
            labelPadding: EdgeInsets.zero,
            tabs: <Widget>[
              Tab(text: "Home"),
              Tab(text: "Repo"),
              Tab(text: "Activity"),
              Tab(text: "Issues"),
            ],
          ),
          actions: <Widget>[
            IconButton(
              icon: Icon(Icons.search),
              onPressed: () {},
            )
          ],
        ),
        body: TabBarView(
          children: <Widget>[
            HomePage(),
            Text("Repo"),
            Text("Activity"),
            Text("Issues"),
          ],
        ),
        drawer: Drawer(
          child: ListView(
            padding: EdgeInsets.zero,
            children: <Widget>[
              DrawerHeader(
                child: Text("Bobson Lin"),
                decoration: BoxDecoration(
                  color: Colors.blueGrey,
                ),
>               ),
              ListTile(
                title: Text("Sign out"),
                onTap: () {},
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// 首頁
class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: ListView(
        children: <Widget>[
          Container(
            child: Divider(
              height: 8.0,
              color: Colors.grey[200],
            ),
            color: Colors.grey[200],
          ),
          ListTile(
            dense: true,
            title: Text("Hackernews Top"),
            trailing: Icon(Icons.chevron_right),
            onTap: () {},
          ),
          Divider(
            height: 0.0,
          ),
          ListTile(
            title: Text(
                "Pre-industrial workers had a shorter workweek than today's"),
            subtitle: Text("by jammygit 2 hours ago | 18 comments"),
            onTap: () {},
          ),
          
          
          ...(中間省略)
          
          ListTile(
            title: Text("google / mediapipe"),
            subtitle: Text("C++   1,505   201"),
            onTap: () {},
          ),
          Divider(
            height: 0.0,
          ),
          ListTile(
            title: Text("MisterBooo / LeetCodeAnimation"),
            subtitle: Text("Java   38,443   6,483"),
            onTap: () {},
          ),
        ],
      ),
    );
  }
}
```
![day4-1.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-1.png?raw=true)


這邊登入後會進入主畫面(MainPage)，根據 TabBar 四個 Tab 分成四個頁面，首頁(HomePage)、倉庫頁(RepoPage)、近況頁(ActivityPage)、問題頁(IssuesPage)

可以注意到，主畫面跟登入頁一樣使用 `Scaffold` 這個 Widget，主要搭配 `MaterialApp` 作使用，它包含了

* Material Design 的排版(如屬性 appBar, bottomAppBar, floatingActionButton...)
* 互動性的元素(如 drawers, snack bars, and bottom sheets)
* 一定程度的可客製化(如 theme, l10n)

對我這個沒什麼設計感的工程師可說是一大便利呢~

一樣起手都是 `StatelessWidget`，上圖特別我用紅色框起來都是可以對應程式碼裡寫的 Widget，

> 目前都只是再拉排版的動作，程式碼看起來不免看起來臃腫
> 之後都是需要慢慢重構，讓程式碼好維護管理的
 

### 欸!? 怪怪的

眼尖(PM?)的同學可能看到有些地方 **怪怪的**...

![柯P-怪怪的](https://attach.mobile01.com/attach/201905/mobile01-c8e9631ce34fb87db32211c73614aa21.jpg)

對，沒錯上面的 TabBar 每個 Tab 根本文字展示不完全，而且離兩邊圖示有些落差...

這時候就可以開啟 Flutter debug 工具或 Devtools (在 VSCode 中可以 Ctrl+Shift+p 直接找 Flutter: Toggle Debug Painting 或 Dart: Open DevTools)

![day4-2.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-2.png?raw=true)

可以清楚看到 Layout 長什麼樣子，加上查詢文件發現有兩處可調整

1. Tab 根本文字展示不完全 => TabBar.labelPadding: EdgeInsets.zero,
2. 離兩邊圖示有些落差 => AppBar.titleSpacing: 0.0

修改完結果

![day4-3.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-3.gif?raw=true)


### 路由 (Route)

好了，到現在我們有兩個頁面了，那要如何作跳轉呢？

稍微用簡單的圖來表示
```
LoginPage (/login) <== [Navigator] ==> MainPage (/home)
```

Flutter 裡在控管 `Route` 切換時會有個中間人 `Navigator`，而控管機制跟 Stack 一樣用 pop/push 操作，詳細可以參考這篇 Medium 文章([Flutter: Push, Pop, Push](https://medium.com/flutter-community/flutter-push-pop-push-1bb718b13c31))


`routes.dart`
``` dart
class GitmeRebornRoutes {
  static const root = "/";
  static const login = "/login";
  static const home = "/home";
}
```

個人會習慣另外定義一個 Routes 類別，來對應路由名稱。

![materialapp_variables](https://bbsonlin.github.io/images/2019-03-04-flutter-whats-in-materialapp/materialapp_variables.png)

接下來就要談到 `MaterialApp` 裡是怎麼設定路由的。從上圖**紅框**中可以看到是影響頁面轉換的屬性。

主要判斷的步驟為

> 1. 若有設定 home 屬性，便認之為 / ([root]根路由)
> 2. 否則查找 routes 屬性，有無對應路由設定。
> 3. 否則會調用 onGenerateRoute，來判斷是否有提供對應路由的回傳。
> 4. 若上述都沒成立，則會調用到 onUnknownRoute。
> 
> (翻譯自 https://api.flutter.dev/flutter/material/MaterialApp-class.html)

接下來，我修改了些原本的程式碼

`main.dart`
[![day4-4.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-4.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/9a64f723e70d09e3f29621ee78c524dfa8ab16e4#diff-fe53fad46868a294b309fc85ed138997)

主要將 home 屬性移除，加上 routes 對應表格並且用 onGenerateRoute 來跳轉根路由到登入畫面(LoginPage)。

`pages/home.dart` & `pages/login.dart`
[![day4-5.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-5.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/9a64f723e70d09e3f29621ee78c524dfa8ab16e4#diff-d3cda190df50f3330b22952d914ba8fe)

再來，登入畫面中的登入按鈕和主畫面中的登出按鈕，當點擊他們時會使用 `Navigator` 導覽至新的頁面。

今日成果

![day4-6.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-6.gif?raw=true)
