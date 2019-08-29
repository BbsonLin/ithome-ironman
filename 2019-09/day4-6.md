###### tags: `第 11 屆 iT 邦鐵人賽`

# 用 Flutter 開發一個 Android App 吧 - Day4 ~ Day6


## Day 4

### 主頁面 - 首頁

`lib/pages/home.dart`
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
              ),
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


這邊登入後會進入主畫面(MainPage)，根據 TabBar 四個 Tab 分成四個頁面，首頁(HomePage)、倉庫頁(RepoPage)、近況頁(ActivityPage)、問題頁(IssuePage)

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

接下來簡單談一下 `MaterialApp` 裡是怎麼設定路由的。從上圖 **紅框** 中可以看到是影響頁面轉換的屬性。

主要判斷的步驟為

> 1. 若有設定 home 屬性，便認之為 / ([root]根路由)
> 2. 否則查找 routes 屬性，有無對應路由設定。
> 3. 否則會調用 onGenerateRoute，來判斷是否有提供對應路由的回傳。
> 4. 若上述都沒成立，則會調用到 onUnknownRoute。
> 
> (翻譯自 https://api.flutter.dev/flutter/material/MaterialApp-class.html)

接下來，我修改了些原本的程式碼

`lib/main.dart`
[![day4-4.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-4.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/9a64f723e70d09e3f29621ee78c524dfa8ab16e4#diff-fe53fad46868a294b309fc85ed138997)

主要將 home 屬性移除，加上 routes 對應表格並且用 onGenerateRoute 來跳轉根路由到登入畫面(LoginPage)。

`lib/pages/home.dart` & `lib/pages/login.dart`
[![day4-5.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-5.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/9a64f723e70d09e3f29621ee78c524dfa8ab16e4#diff-d3cda190df50f3330b22952d914ba8fe)

再來，登入畫面中的登入按鈕和主畫面中的登出按鈕，當點擊他們時會使用 `Navigator` 導覽至新的頁面。

今日成果

![day4-6.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day4-6.gif?raw=true)



---


## Day 5

### 登入/登出

登入按下登入按鈕後的時候，需要有一個 Loading 的 Modal，而在 Flutter 所提供的 Material 元件裡找不到能直接使用的，這時候就到 Dart 的套件管理網站 https://pub.dev/flutter 去找找有沒有別人寫好的套件吧~

滿幸運的是，我找到一個名為 [flutter_progress_hud](https://pub.dev/packages/flutter_progress_hud) 的套件，整體狀態有 90 分呢~  

照著文件範例修改一下 `lib/pages/login.dart`

[![day5-1.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day5-1.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/eb9c76de098952ee8049288abec13d223b484c03#diff-9f0df600d112c287183a6aa77167120d)

登出的話需要跳出一個讓使用者確定登出的 AlertDialog。

修改 `lib/pages/home.dart`

[![day5-2.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day5-2.jpeg?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/eb9c76de098952ee8049288abec13d223b484c03#diff-d3cda190df50f3330b22952d914ba8fe)

> 注意:  
> * `pubspec.yaml` dependencies 裡面需要新增 `flutter_progress_hud: ^1.0.2` 這套件。
> * 參考: [官方文件 - Using packages](https://flutter.dev/docs/development/packages-and-plugins/using-packages)

--

接下來，我們把後續的主頁面的排版先架構出來吧。

在主頁面時可以看到笨笨的寫了好幾個 `ListTile` 和 `Divider`，這樣個作法也許在作 Demo 時還可以使用，但實際上若有數十個、數百個不就完了嗎？

回想一下 Day3 講的 `Think declaratively`，其他頁面可以稍微改良一下，我們可以先暫時宣告變數當作狀態，接著 `Widget` 在從變數中填入對應要顯示的值。

什麼意思呢~ 接著往下看。


### 倉庫頁(RepoPage)

`lib/pages/repo.dart`
``` dart
import 'package:flutter/material.dart';

class RepoPage extends StatelessWidget {
  final List repoList = [
    {
      "title": "BbsonLin/gitme_reborn",
      "description": "No description provided.\n\n★ 0",
      "lang": "● Dart"
    },
    {
      "title": "BbsonLin/ithome-ironman",
      "description": "No description provided.\n\n★ 0",
      "lang": ""
    },
    {
      "title": "BbsonLin/flask-request-logger",
      "description":
          "A Flask extension for recording requests and responses into database\n\n★ 3",
      "lang": "● Python"
    },
  ];

  @override
  Widget build(BuildContext context) {
    return Container(
      child: ListView.separated(
        itemCount: repoList.length,
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            title: Text(repoList[index]["title"]),
            subtitle: Text(repoList[index]["description"]),
            trailing: Text(repoList[index]["lang"]),
            isThreeLine: true,
            contentPadding: EdgeInsets.all(16.0),
            onTap: () {},
          );
        },
        separatorBuilder: (BuildContext context, int index) =>
            const Divider(height: 0.0),
      ),
    );
  }
}
```

宣告 `final List repoList` 來暫時當作未來會拿到的資料包(狀態)，而且在 UI 上面採用 `ListView.separated` 這方法可以更快速達成想要的畫面。

程式碼看起來是不是更簡潔有力點了呢？

![day5-3.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day5-3.png?raw=true)


### 近況頁(ActivityPage) 與 問題頁(IssuePage)

那麼這兩個頁面其實就是依樣畫葫蘆，偷懶一下就不貼程式碼上來了~🤪  
詳細 commit 可以點擊成果 GIF 觀看~

> 注意:  
> 這邊雖然使用了變數儲存狀態，但還是使用 `StatelessWidget`，想要達成交互式(Reactive) UI 需要使用 `StatefulWidget`，後續會談到~


今日成果

[![day5-4.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day5-4.gif?raw=true)](https://github.com/BbsonLin/gitme_reborn/commit/6a837c743d9a792552ff32003171b0023c68d756)



---


## Day 6

### 導覽選單(Drawer)

接下來完整導覽選單的部份，最上方 Header 的部份使用 [`UserAccountsDrawerHeader`](https://api.flutter.dev/flutter/material/UserAccountsDrawerHeader-class.html)，這是 Flutter 幫我們打造的 material Widget，來符合一致的 Material 風格。

``` dart
UserAccountsDrawerHeader(
  decoration: BoxDecoration(
    color: Colors.blueGrey,
  ),
  accountName: Text("Bbson Lin"),
  accountEmail: Text("bobson801104@gmail.com"),
  currentAccountPicture: CircleAvatar(
    backgroundImage: NetworkImage(
      "https://avatars2.githubusercontent.com/u/18156421?s=400&u=1f91dcf74134827fde071751f95522845223ed6a&v=4",
    ),
  ),
  otherAccountsPictures: <Widget>[
    Icon(Icons.edit, color: Colors.white),
  ],
),
```

`UserAccountsDrawerHeader` 提供了幾個人性化的屬性，像是 `accountName`、`accountEmail`、`currentAccountPicture` ...等，只要將這些屬性填上你想要的 Widget 就行了。

![day6-1.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day6-1.jpeg?raw=true)

導覽選單部份是用幾個 `ListTile` 做出來的，我們在 title 屬性多加了圖示。

``` dart
ListTile(
  title: Row(
    children: <Widget>[
      Icon(Icons.power_settings_new),
      SizedBox(width: 24.0),
      Text("Sign out"),
    ],
  ),
  onTap: () async {
    await showDialog(
      context: context,
      barrierDismissible: false,
      builder: (context) => AlertDialog(
        content: Text("Are you sure to exit current account."),
        actions: <Widget>[
          FlatButton(
            child: Text("Cancel"),
            onPressed: () => Navigator.pop(context),
          ),
          FlatButton(
            child: Text("OK"),
            onPressed: () => Navigator.pushNamedAndRemoveUntil(
                context, "/login", ModalRoute.withName('/')),
          ),
        ],
      ),
    );
  },
),
```

其實目前看起來已經是有點小小的冗長(30行)，若是好幾個都這樣寫，不光是看起來難過，之後維護修改起來也不容易。

解決的方法，就是自己再封裝 Widget，之後可以重複利用，如同 `UserAccountsDrawerHeader` 一般。

`lib/components/drawer_tile.dart`
``` dart
import 'package:flutter/material.dart';

class DrawerTile extends StatelessWidget {
  const DrawerTile({
    Key key,
    this.icon,
    this.text = "",
    this.onPressed,
  }) : super(key: key);

  final Icon icon;
  final String text;
  final Function onPressed;

  @override
  Widget build(BuildContext context) {
    return ListTile(
      title: Row(
        children: <Widget>[
          icon,
          SizedBox(width: 24.0),
          Text(text),
        ],
      ),
      onTap: onPressed ?? () {},
    );
  }
}
```

個人習慣放自定義的 Widget 在 components 目錄裡面，`DrawerTile` 單純的封裝 `ListTile` 並把一些屬性定義出來 `icon`、`text`、`onPressed`。

![day6-2.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day6-2.png?raw=true)


### 搜尋頁(Search Page)


搜尋頁面 Flutter 內也有提供 [`showSearch`](https://api.flutter.dev/flutter/material/showSearch.html) 及搭配的 [`SearchDelegate`](https://api.flutter.dev/flutter/material/SearchDelegate-class.html)。

渲染出來的頁面是符合 [Material Design - Expandable search](https://material.io/archive/guidelines/patterns/search.html#search-in-app-search) 的設計。

![day6-3.jpeg](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day6-3.jpeg?raw=true)

首先，要在按下搜尋按鈕時調用 `showSearch`。

`lib/pages/search.dart`
``` dart
import 'package:flutter/material.dart';

enum SearchTypes {
  repos,
  users,
}

// Use(Extends) SearchDelegate for Search Page
class GitmeRebornSearchDelegate extends SearchDelegate {
  SearchTypes _searchType = SearchTypes.repos;

  @override
  ThemeData appBarTheme(BuildContext context) => Theme.of(context); // AppBar 主題

  @override
  List<Widget> buildActions(BuildContext context) { // AppBar 右側
    return [
      IconButton(
        icon: Icon(Icons.clear),
        onPressed: () {
          query = "";
        },
      ),
      PopupMenuButton(
        onSelected: (SearchTypes type) {
          _searchType = type;
          showSuggestions(context);
        },
        itemBuilder: (BuildContext context) {
          return [
            CheckedPopupMenuItem<SearchTypes>(
              value: SearchTypes.repos,
              checked: _searchType == SearchTypes.repos,
              child: const Text("Search Repos"),
            ),
            CheckedPopupMenuItem<SearchTypes>(
              value: SearchTypes.users,
              checked: _searchType == SearchTypes.users,
              child: const Text("Search Users"),
            ),
          ];
        },
      ),
    ];
  }

  @override
  Widget buildLeading(BuildContext context) { // AppBar 左側
    return IconButton(
      icon: Icon(Icons.arrow_back),
      onPressed: () {
        close(context, null);
      },
    );
  }

  @override
  Widget buildResults(BuildContext context) { // 搜尋結果
    return SearchRepoResult();
  }

  @override
  Widget buildSuggestions(BuildContext context) { // 搜尋建議
    if (query == "") {
      switch (_searchType) {
        case SearchTypes.repos:
          return Container(
            width: MediaQuery.of(context).size.width,
            padding: EdgeInsets.all(32.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.center,
              mainAxisSize: MainAxisSize.max,
              children: <Widget>[
                Text(
                  "Search Repos",
                  style: TextStyle(
                    fontStyle: FontStyle.italic,
                    color: Theme.of(context).textSelectionColor,
                  ),
                ),
              ],
            ),
          );
        case SearchTypes.users:
          return Container(
            width: MediaQuery.of(context).size.width,
            padding: EdgeInsets.all(32.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.center,
              mainAxisSize: MainAxisSize.max,
              children: <Widget>[
                Text(
                  "Search Users",
                  style: TextStyle(
                    fontStyle: FontStyle.italic,
                    color: Theme.of(context).textSelectionColor,
                  ),
                ),
              ],
            ),
          );
        default:
      }
    }
    return Center(child: Text("Search for $query ..."));
  }
}

class SearchRepoResult extends StatelessWidget {
  ... (略)
}
```

直接看程式碼可能有點霧煞煞，我們來看渲染出來的頁面，與其對應的關係。

![day6-4.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day6-4.png?raw=true)

可以看得出 `GitmeRebornSearchDelegate.buildActions` 就是建構紅框 actions，依此類推 leading, suggestions 和 results 。

> 注意:  
> * `SearchDelegate` 只是個抽象類別，本身沒實現 buildActions 等函數，一定需要自己實現搜尋的 Delegate。
> * 調用 `showSearch` 時要帶入 delegate 屬性的是自己實現的 `GitmeRebornSearchDelegate`，並非 `SearchDelegate`，否則會跳出紅色錯誤頁面。
> * Flutter 為何會這麼設計，原因之一無非是更彈性一點，你可以建構自己的 actions 區，又不失風格一致性。

--

今日成果

![day6-5.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day6-5.gif?raw=true)


*參考*

* [Medium - Implementing search in Flutter](https://medium.com/flutterpub/implementing-search-in-flutter-17dc5aa72018)
* [Flutter YouTube - Flutter's Search Support (The Boring Flutter Development Show, Ep. 10)](https://youtu.be/Wm3OiFBZ2xI)