###### tags: `第 11 屆 iT 邦鐵人賽`

# 用 Flutter 開發一個 Android App 吧 - Day1 ~ Day3


## Day 1.

### 移動端的開發

* Android/iOS - Java & Kotlin/Objective-C & Swift
    * 優勢: 
        * 用原生的語言開發，效能很好
        * 由於推出時間很久了，非常穩定以及能搜尋到的資源也很多
    * 劣勢:
        * 兩種平台須寫兩份不同語言的程式碼，開發及維護上需要較多人力資源
* React Native(RN) - Javascript
    * 優勢:
        * 只須一種語言能編成兩種平台的 App
        * 用前端語言開發，對於前端開發者較好上手
        * 推出時間有五年了，整體框架算穩定以及社群也較多
    * 劣勢:
        * 需要先學習過 React 會比較好
* Flutter - Dartlang
    * 優勢:
        * 只須一種語言能編成兩種平台的 App
        * Dartlang 為 Java-Like 的語言，學習過 Java 會滿好上手的
        * Dartlang 是能需要編譯的，效能上比較好
    * 劣勢:
        * 推出時間短(到現在兩年半)，目前版本迭代速度滿快的
* NativeScript - Javascript
    * 優勢:
        * 只須一種語言能編成兩種平台的 App
        * 用前端語言開發，與幾個現在主流的前端框架(Angular、Vue.js)有作整合
    * 劣勢:
        * 近一年來好像才比較穩定(?)(至少我是最近才有看到它跟 Vue.js 整合)


>備註:  
> * `Flutter` 2018 5月釋出 Beta 3，經過兩版 Preview，在 2018 12月推出 1.0.0。
> * `Flutter` 2019 7月推出 1.7.8，經過 3 個 stable 版本。
> * 筆者大概是 2018 5、6月時，在 `Flutter` 與 `RN` 兩個框架中作選擇，不過當初半路出家學習前端技術，較為熟悉的前端框架是 `Vue.js`，所以沒選擇 `RN`，而 `NativeScript-Vue` 還沒出現。
> * 當初選擇 `Flutter`，另外一個原因是 `Android` 是由 `Google` 在稱的手機作業系統，所以覺得會進 [Google Graveyard](https://killedbygoogle.com/) 機率應該不高才對 ... 🤪
> * **其實還有很多其他的開發工具(框架)，而選擇工具為個人因素決定，並無絕對的好壞，只要能達成目的，且自己用的上手最重要。(❤ ✌ 😃 Love & Peace)**


### Dart(Dartlang) 語言的學習

還未接觸 `Flutter` 前，我是完全沒聽過這語言。  
而在決定要學習 `Flutter` 後，Dartlang 這門語言就一定得學習了。  
因為我個人工作經驗是有接觸過 Java 與 Python，在加上有自學 JavaScript，在學習程式語言上其實有很多概念是相通的，所以學習 Dartlang 上並沒有太大的負擔。

我的學習來源是 YouTube 及 Dartlang 官方文件，筆者也有整理幾篇文章在 [**個人部落格**](https://bbsonlin.github.io/tags/dart/)，還有發表在 [**掘金個人專欄**](https://juejin.im/user/59cc3f02f265da06700af973/posts?sort=popular) 上，有興趣的人可作參考；另外也有其他鐵人賽的參賽選手大家也可以去看看~~ 筆者在這門語言上面就不多花筆墨了~


### Flutter 的學習

其實到現在 `Flutter` 的學習資源已經滿多的了，[**官方文件**](https://flutter.dev) 提供的內容滿值得細讀的，對了解框架上有一定程度的幫助。

另外 `Flutter` 官方維護的 YouTube 頻道也有很多影片資源，筆者從這上面學到很多東西，也可看得出來這團隊的用心。


### 30 天規劃

* **[1-2 篇]** 前言 & 簡單介紹
* UI 部份 [10~15 篇]
* API 串接 [5~10 篇]
* Store(State) 狀態管理 [5~8篇]
* **[31+ 篇]** Enhancement & Others

---

## Day 2. 

### 開發環境

> 作業系統(OS): Ubuntu Desktop 18.04.2 LTS  
> 編輯器(Editor): VS Code  
> Flutter 版本: stable (1.7.8+hotfix.4) 
> Dart 版本: 2.4.0
> Android SDK: 28  
> Android 版本: 7.1.1  
> 手機裝置: HTC U11  
> 手機投影工具: [scrcpy](https://github.com/Genymobile/scrcpy)

Flutter 開發環境安裝 - https://flutter.dev/docs/get-started/install


### 開新專案

直接使用 `flutter create` 指令來開始新的專案。

``` bash
$ flutter create .
Creating project ....
  android/app/src/debug/AndroidManifest.xml (created)
  android/app/src/profile/AndroidManifest.xml (created)
  android/app/src/main/AndroidManifest.xml (created)
  
  ...(中間略過)
  
  pubspec.yaml (created)
  README.md (created)
  ios/Runner.xcodeproj/project.pbxproj (created)
  ios/Runner/main.m (created)
  ios/Runner/AppDelegate.m (created)
  ios/Runner/AppDelegate.h (created)
  .gitignore (created)
Running "flutter pub get" in gitme_reborn...                        3.9s
Wrote 66 files.

All done!
[✓] Flutter is fully installed. (Channel stable, v1.7.8+hotfix.4, on Linux, locale zh_TW.UTF-8)
[✓] Android toolchain - develop for Android devices is fully installed. (Android SDK version 28.0.3)
[✓] Android Studio is fully installed. (version 3.4)
[✓] VS Code is fully installed. (version 1.35.1)
[✓] Connected device is fully installed. (1 available)

In order to run your application, type:

  $ cd .
  $ flutter run

Your application code is in ./lib/main.dart.
```

### 文件目錄介紹  
  ![day2-1.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day2-1.png?raw=true)
  
### 跑起 App

  我都是用 VSCode 直接使用 `F5` 跑起 Debug 模式；另一個方式可以在命令視窗打 `flutter run` 也行。  
  結果會在手機上出現一個 Flutter 預設創建的 Count App
  
  ![day2-2.gif](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day2-2.gif?raw=true)
  
### Gitme Reborn 發想

  目前 Google Play Store 上已經有滿多連接 GitHub 的手機 App，其中有一個是由 flutterchina 推出的 [Gitme](https://flutterchina.club/app/gm.html)。  
  不過作者有在 [Gitme repo](https://github.com/flutterchina/gitme) 上提到沒備份到之前的原始程式碼且也不打算繼續維護，我覺得還滿可惜的。  
  然而作者還留有 Gitme 的設計稿，在加上我覺得其實這 App 也還算滿好用的，因此引發我想創建一個開源、並可以拿來實做 Flutter App 的專案；正好 IT 鐵人賽即將開賽，也可以藉由此機作個專案心路歷程的紀錄。  
  **希望能堅持到最後囉~~ 😆**
  
### Gitme Reborn UI 流程
  
  ![day2-3.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day2-3.png?raw=true)

希望 30 天可以達成上圖的 70%-80% 的功能，有些 UI 或功能會先考量簡易版，之後有時間可以再慢慢加上。


---


## Day 3.

### 進入點

`main.dart`
``` dart
import 'package:flutter/material.dart';
import 'package:gitme_reborn/pages/login.dart';

void main() => runApp(GitmeRebornApp());

class GitmeRebornApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blueGrey,
      ),
      home: LoginPage(),
    );
  }
}
```

![day3-1.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day3-1.png?raw=true)

基本上跟一般程式語言一樣進入點在 `void main` 這個函數，這裡頭直接調用 Flutter SDK 所提供的 `runApp` 函數，去啟動整個 Flutter App。

我直接將 Flutter 預設創建出來的 MyApp 稍微作些改寫，並刪除些不要的部份，剩下 20 行不到。

而這裡 `runApp` 函數我們帶入 `GitmeRebornApp` 這個 `Widget` 給它(`StatelessWidget` 表示他是沒有附加狀態的 `Widget`)，後續 Flutter 都會幫你處理好。

注意，這裡使用 `MaterialApp`，它直接封裝了 Google 推出的 Material Design 的一些設定。如果想使用 Apple iOS 風格設計可使用 `CupertinoApp` 來建構，基本上直接使用 `MaterialApp`就有得玩了~


### 登入頁面

`pages/login.dart`
``` dart
import 'package:flutter/material.dart';

class LoginPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Login"),
      ),
      body: SingleChildScrollView(
        child: Column(
          children: <Widget>[
            Container(
              padding: EdgeInsets.symmetric(horizontal: 24.0, vertical: 16.0),
              child: TextFormField(
                decoration: const InputDecoration(
                  prefixIcon: Icon(Icons.person),
                  labelText: "Name *",
                  hintText: "Your Github account username",
                ),
              ),
            ),
            Container(
              padding: EdgeInsets.symmetric(horizontal: 24.0, vertical: 16.0),
              child: TextFormField(
                decoration: const InputDecoration(
                  prefixIcon: Icon(Icons.lock),
                  suffixIcon: Icon(Icons.remove_red_eye),
                  labelText: "Password *",
                  hintText: "Your Github account password or ...",
                ),
              ),
            ),
            SizedBox(
              height: 52.0,
            ),
            SizedBox(
              width: MediaQuery.of(context).size.width - 48.0,
              height: 48.0,
              child: RaisedButton(
                child: Text("Login"),
                onPressed: () {},
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

![day3-2.png](https://github.com/BbsonLin/ithome-ironman/blob/master/2019-09/images/day3-2.png?raw=true)

接下來直接看看我們在 `GitmeRebornApp.home` 裡面自己寫的的 `LoginPage`。

基本上所在撰寫新的 `Widget` 時都是以 `StatelessWidget` 起手，如果頁面會使用到一些狀態改變(使用者操作、讀取數據...等等事情發生時)，才會用到 `StatefulWidget`



### 欸!? 暫停一下

> blablabla 貼了一大堆程式碼，直接就進入業配(頁面)主題阿~~  
> 根本無法承受阿~~~  
> 一堆概念根本不知道怎麼來的阿...

哦哦哦~ 我知道... 畢竟一開始我也是這樣的...

以下一些概念，是我透過官方文件或影片理解到的:

* Flutter 世界裡一切的東西都是 Widget。  
  所以在開發 Flutter App，很大部分是在建構 Widget Tree(Widget 樹)，而大部份的 UI 功能會操作這些 Widget 也就夠了。

* Flutter 很講重建構 UI 的方式應為陳述式(宣告式)(Declarative UI)的思考方式，相對於以前 Android 或 Java Swing 的命令式(Imperative UI)。  
  ![Start thinking declaratively](https://flutter.dev/assets/development/data-and-backend/state-mgmt/ui-equals-function-of-state-54b01b000694caf9da439bd3f774ef22b00e92a62d3b2ade4f2e95c8555b8ca7.png)
  所以在開發 Flutter App，如果仔細觀察，其實可以很容易的將規劃的 UI，直接對應到程式碼。  
  所有事件(操作)發生都會改變狀態，而開發者應該直接思考的是  
  
  > **狀態如何輸入** 會 **輸出怎樣的畫面**
  
  ，也就是上圖的 f 函數。

* **[深入概念]** Flutter 內部在構件及渲染畫面的步驟是  

  `Widget Tree => Element Tree => RenderObject Tree`

  可以大致想像 Element Tree 是中間人，它必須知道 Widget Tree 做了什麼狀態改變，它需要怎調整(重建) Element Tree，另外它還要通知 RenderObject Tree 去渲染畫面。


好了，到這裡希望大家能多多少少理解到 Flutter 開發 UI 的概念，後續在看程式碼上也比較容易理解~

*參考*

* [Flutter.dev - Start thinking declaratively](https://flutter.dev/docs/development/data-and-backend/state-mgmt/declarative)
* [Flutter YouTube - Flutter Widgets 101 Ep. 1](https://youtu.be/wE7khGHVkYY)
* [Flutter实战 - 14.2 Element与BuildContext](https://book.flutterchina.club/chapter14/element_buildcontext.html)


