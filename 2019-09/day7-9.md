###### tags: `第 11 屆 iT 邦鐵人賽`

# 用 Flutter 開發一個 Android App 吧 - Day7 ~ Day9


## Day 7

### 頁面刷新(Page Refresh) 和 StatefulWidget

今天來完成一下頁面刷新的部份，先拿 Day 5 完成的倉庫頁(RepoPage)來作些改變。

![day7-1.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-1.png?raw=true)

首先把原本得 `Container` 替換成 `RefreshIndicator` 這個 Widget，而使用`RefreshIndicator` 有一個一定要填入的屬性 `onRefresh`。

在 `onRefresh` 中，我們簡單的在先前宣告的 repoList 變數加上新的值，模擬頁面刷新時有新的資料加入。

但事情總不像我這憨人想像的如此容易...

![day7-2.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-2.gif?raw=true)

![day7-3.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-3.jpeg?raw=true)

咦，在 Debug Console 中，我們卻實實在在地看到 repoList 裡有新增 `{"title": "BbsonLin/new-item", "description": "", "lang": ""}` 阿，這是怎回事呢？

原來，只是 Flutter 並不知道你的 repoList 要立即對應、更新你的頁面(因為我們這還是用 `StatelessWidget`...)，那麼要怎樣作才能讓 Flutter 達成這效果呢？

沒錯，聰明的同學一下就知道還有 `StatefulWidget` 的存在。
下面我們就著手把 `StatelessWidget` 變成 `StatefulWidget` 吧~

> 小提醒:
> 
> 如果是使用 VSCode 作開發的話在 class 上方會出現燈泡點選之後，選擇 `Convert to StatefulWidget`，可以快速的變成 `StatefulWidget` 的形式喔。
>
> ![day7-4.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-4.png?raw=true)
> 
> 這方式節省非常多時間，也是為何我都會從 `StatelessWidget` 起手的原因。
 
[![day7-5.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-5.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/cfffac4df9a4c272ee4bd570a7e52c5f77b7730b#diff-ba48edd1b979d11d2fd7d4250ea4a6eb)

變成 `StatefulWidget` 後有另一個重點，就是要使用 `setState` 這個函數，要變換的狀態必須要在這函數裡面才行。


### 欸!? 修一些 Bug 吧

對，沒錯，我想有些人看出不少的小問題

所以這邊我想暫停一下，花一些篇幅來作 Fix bug 和 Enhancement

這邊只敘述修改的項目和我找到哪些參考來解決他們，詳細的程式碼都可以 **點選圖片連結觀看** 喔~

1. 首頁的重構(Home Page - Refactor)
    
    > 修改的項目:
    > * 一堆的 ListTile 改用 ListTile.divideTiles 取代
    > * 加入頁面刷新
    > * 改成 StatefulWidget

    [![day7-6.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-6.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/39a7e379be23df0142055828c7a49e67e7707762)

2. Main Page - Scrollbar
    
    用 Flutter 內建的 [`Scrollbar`](https://api.flutter.dev/flutter/material/Scrollbar-class.html) Widget，滿簡單使用的，詳細可以參看文件。

3. Main Page - Circle Avatar Button

    在主畫面的左上按鈕，UI 設計中為頭像按鈕，於是我另外創建了 `CircleAvatarButton` ，基本上就是將 `IconButton`中的 icon 換成 `CircleAvatar`。

> 小提醒:
> 
> * 在重構時我有用到 Dart 2.3 才引入的 Spread Operator，用法跟 JavaScript 的類似，詳細可以參考 [Medium - What’s New In Dart 2.3?](https://medium.com/flutter-community/whats-new-in-dart-2-3-1a7050e2408d)

--

今日成果

[![day7-7.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day7-7.gif?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/c75411b3de0a840f82236f98a242ab04b758bb57)

*參考*

* [Medium - Adding Swipe To Refresh to Flutter app](https://medium.com/flutterpub/adding-swipe-to-refresh-to-flutter-app-b234534f39a7)
* [Reddit - How do you open a drawer in a scaffold using code?](https://www.reddit.com/r/FlutterDev/comments/7yma7y/how_do_you_open_a_drawer_in_a_scaffold_using_code/)
* [StackOverflow - Flutter rounded profile image appBar](https://www.reddit.com/r/FlutterDev/comments/7yma7y/how_do_you_open_a_drawer_in_a_scaffold_using_code/)

---

## Day 8.

### 欸!? 還沒改完?

恩... 越看越多東西需要修改一下...

那麼接續昨天的議題。

4. Login Page - password text hide & text show/hide switch

    > 修正的項目:
    > * 密碼隱藏輸入字元
    > * 可切換密碼字元 顯示/隱藏
    
    隱藏輸入字元可藉著 TextFormField.obscureText 屬性來調整。
    
    切換密碼字元 顯示/隱藏，參考 [YouTube - Flutter - toggle password visibility](https://youtu.be/OwDFVQc-WOc?t=244)
    
5. Login Page - loading modal
    
    > 修正的項目:
    > * Loading 底下有雙黃線
    > * 按下登入紐收起鍵盤
    
    Loading 字樣有雙黃線，參考了 [StackOverflow - Yellow lines under Text Widgets in Flutter?](https://stackoverflow.com/questions/47114639/yellow-lines-under-text-widgets-in-flutter) 及相關 Issue [Text in Hero red when animating #30647](https://github.com/flutter/flutter/issues/30647)
    
    如何在按下登入紐取消鍵盤呢？參考 [StackOverflow - How can I dismiss the on screen keyboard?](https://stackoverflow.com/questions/44991968/how-can-i-dismiss-the-on-screen-keyboard)
    
6. Login Page - login button color

    直接在 `lib/main.dart` 中，`theme` 屬性中添加 `ThemeData.buttonTheme`，正也是 `MaterialApp` 好用的地方。

--
修改後成果

[![day8-1.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day8-1.gif?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/f5cc0e674d54da2846ddeb13974f0de76ad8b13b)
> 點選 GIF 可直接看 Commit


### 個人頁面(Profile Page)

好囉~ 現在回到 UI 開發上面。

![gitme user.jpeg](https://raw.githubusercontent.com/flutterchina/gitme/master/imgs/user.jpeg)

在個人頁面的資訊區域(上半部頭貼、自介、地點等等...)，可以視為一個擴增版的 AppBar。

再加上下半部會有大量可捲動的資訊區域(Repos、Stars ...)，這時候我們可以使用 Flutter 中一些特別的 Widget； `CustomScrollView` 和 `SliverXXX` 系列來達成此功能。

完全聽不懂嗎?
沒關係，先作一個簡單的 SliverAppBarDemo 頁面(參考 [Flutter实战 - 6.5 CustomScrollView](https://book.flutterchina.club/chapter6/custom_scrollview.html))

``` dart
import "package:flutter/material.dart";

class SliverAppBarDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomScrollView(
        slivers: <Widget>[
          SliverAppBar(
            pinned: true,
            expandedHeight: 250.0,
            flexibleSpace: FlexibleSpaceBar(
              centerTitle: true,
              title: Container(
                padding: EdgeInsets.all(8.0),
                child: Text('SliverAppBar Demo'),
                color: Colors.black54,
              ),
              background: Image.network(
                "https://images.unsplash.com/photo-1515975325863-a4ceb4b7d6c0?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1225&q=80",
                fit: BoxFit.cover,
              ),
            ),
          ),
          SliverPadding(
            padding: const EdgeInsets.all(8.0),
            sliver: SliverFixedExtentList(
              itemExtent: 50.0,
              delegate: SliverChildBuilderDelegate(
                (BuildContext context, int index) {
                  return Container(
                    alignment: Alignment.center,
                    color: Colors.lightBlue[100 * (index % 9)],
                    child: Text('list item $index'),
                  );
                },
                childCount: 50,
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

結果

![day8-2.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day8-2.gif?raw=true)

這樣應該稍微有點概念了吧~ 接下來就看看實際如何用在 ProfilePage 裡吧~

`lib/pages/profile/profile.dart`
``` dart
import "package:flutter/material.dart";
import 'package:gitme_reborn/components/profile/profile_info.dart';

class ProfilePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      child: Scaffold(
        body: CustomScrollView(
          slivers: <Widget>[
            SliverAppBar(
              title: Text("BbsonLin"),
              pinned: true,
              expandedHeight: 250.0,
              flexibleSpace: FlexibleSpaceBar(
                background: Column(
                  mainAxisAlignment: MainAxisAlignment.end,
                  children: <Widget>[
                    ProfileInfo(
                      avatarUrl:
                          "https://avatars2.githubusercontent.com/u/18156421?s=400&u=1f91dcf74134827fde071751f95522845223ed6a&v=4",
                      name: "Bobson Lin",
                      location: "New Taipei City, Taiwan",
                    ),
                    SizedBox(height: 8.0),
                    TabBar(
                      labelPadding: EdgeInsets.zero,
                      tabs: <Widget>[
                        Tab(text: "Repos"),
                        Tab(text: "Stars"),
                        Tab(text: "Followers"),
                        Tab(text: "Following"),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            SliverFillRemaining(
              child: TabBarView(
                children: <Widget>[
                  Text("Repos"),
                  Text("Stars"),
                  Text("Followers"),
                  Text("Following"),
                ],
              ),
            ),
          ],
        ),
      ),
      length: 4,
    );
  }
}
```

個人頁面的資訊區域(上半部)，額外拉出來寫成 `ProfileInfo`


`lib/components/profile/profile_info.dart`
``` dart
import "package:flutter/material.dart";

class ProfileInfo extends StatelessWidget {
  const ProfileInfo({
    Key key,
    @required this.avatarUrl,
    @required this.name,
    this.bio,
    this.location,
  }) : super(key: key);

  final String avatarUrl;
  final String name;
  final String bio;
  final String location;

  @override
  Widget build(BuildContext context) {
    TextTheme _primaryTextTheme = Theme.of(context).primaryTextTheme;

    return Column(
      children: <Widget>[
        CircleAvatar(
          radius: 36.0,
          backgroundImage: NetworkImage(avatarUrl),
        ),
        SizedBox(height: 8.0),
        Text(
          name,
          style: _primaryTextTheme.subtitle,
        ),
        SizedBox(height: 6.0),
        Text(
          bio ?? "No bio yet",
          style: _primaryTextTheme.body1,
        ),
        SizedBox(height: 6.0),
        Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Icon(
              Icons.location_on,
              size: _primaryTextTheme.caption.fontSize,
              color: _primaryTextTheme.caption.color,
            ),
            SizedBox(width: 4.0),
            Text(
              location,
              style: _primaryTextTheme.caption,
            ),
          ],
        ),
      ],
    );
  }
}
```


> 小提醒:
> 
> * 在 Material Design 中，針對頂部應用欄 (App Bars: top) 的行為有些規範，參考: [App Bars: top - Behavior](https://material.io/components/app-bars-top/#behavior)
> * `Sliver` 系列，是較進階的渲染方式。出現原因主要是解決，有大量資訊需要用捲動顯示時設備資源浪費的問題；試想一下，如果有 100、1000 個需要條列的資料，如果一次就要渲染，性能較差的手機可能就負荷不了了，效能好的手機就算能作到，也會影響使用者的使用體驗。
> * 想深入了解的同學不妨看看參考中的 YouTube 影片。

--
成果

[![day8-3.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day8-3.gif?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/540a1e59526258627c2bf0062c309babf4f54cbd)


今天先到這，明天在接續完成個人頁面下半部吧~

參考:

* [YouTube - Slivers Explained - Making Dynamic Layouts (The Boring Flutter Development Show, Ep. 12)](https://youtu.be/Mz3kHQxBjGg)
* [程式設計之美 - Material Design 2設計規範（五十六）：頂部應用欄](https://beautyofprogram.com/2018/08/material-design-2%e8%a8%ad%e8%a8%88%e8%a6%8f%e7%af%84%ef%bc%88%e4%ba%94%e5%8d%81%e5%85%ad%ef%bc%89%ef%bc%9a%e9%a0%82%e9%83%a8%e6%87%89%e7%94%a8%e6%ac%84/)
