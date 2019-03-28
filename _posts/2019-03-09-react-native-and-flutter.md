---
layout: post
title: "React Native and Flutter"
cateogiries: []
tags: [flutter,react-native,cross platform framework, mobile development]
author: Dooyoung Gi
---


<p>&nbsp;When I first knew about React Native 2 yaers ago, it was fascinating that I could develop iOS application using almost the same codebase and especially reload/hot-reload functionalities which made significant improvement on UI development cycle.
</p>
<p>
&nbsp;One of the best benefits of using a cross platform framework is flexible accessibility between different platforms with just one language. But nothing can be perfectly satisfied, always comes with some losses. While I’m using React Native, I’ve been faced many problems such as platform conflicts and limited support for some simple components. Essentially these kinds of situation are inevitable if you’re using cross-platform framework, not story about only React Native. Nevertheless as the demands of these tools are increasing for many benefits, various frameworks are out there. Recently, I got a chance to learn another cross platform framework ‘Flutter’ developed by Google. I made a simple demo application which requires fairly tricky animation effects.
</p>
<p>
&nbsp;This is the demo app that I made using Flutter. The main goal of this demo is implementing these features below,</p>

* Horizontal scroll view to show top deals available.
* Capture && Drop animation effects for ‘add to favorites’
* Vertical drawer to see favorited items.
* Hero(Shared elements) animation to navigate to details screen.

<img 
  src="/assets/reward_demo.gif" 
  width="270" 
  height="560" />
<br>
[The design is made from my colleage](https://medium.com/fm-stories/how-developers-and-designers-can-collaborate-using-flutter-35b3e49046ca)

<p>
&nbsp;It’s quite surprising how I quickly implemented these animations just in few days using Flutter even though Dart was also new to me. (*Flutter uses Dart language). So I came up with the idea of writing about functionalities that I found out useful in Flutter and compare it to React Native. I still have lots of things to try out in Flutter and this comparison might be seen not fair enough. So, this is just my overall thoughts on these two frameworks and hoping that it helps somebody who is thinking to use React Native or Flutter (even any other cross-platform framework) for their product in the future.
</p>
<br>
  
### How do they work behind

| Framework    | Language     | Environment                              |
|:-------------|:-------------|:-----------------------------------------|
| Flutter      | Dart         | DartVM                                   |
| React Native | JavaScript   | JavaScriptVM(JavaScriptCore, Chrome V8)  |

<p>&nbsp;Flutter uses Dart and runs on DartVM. Basically DartVM has two ways to execute Dart code. One is JIT and the other one is AOT snapshots. In debug mode, Flutter creates persistent process which uses state caches to re-compile only the part which is changed, and this comes to as hot-reload in Dart JIT. In profile and release mode, Flutter uses AppAOT snapshots which shows faster startup time and best performance without warm-up time.</p>

<p>&nbsp;In React Native, it uses JavaScript and runs on Chrome engine for debugging mode. Since JavaScript is dynamically typed language, it’s best for utilizing JIT but not applicable to AOT. So RN uses JavaScriptCore to interpret JavaScript. These engines will create bundle file written in JavaScript and pass it to ‘bridge’ which gives interface between native platforms and JavaScriptVM. JavaScript thread which got this signal, started issuing work to do from its bundle code.</p>

<br>

### Module Dependencies
&nbsp;Cross platform framework requires external modules (called ‘plugins’ in Flutter) to extend its functionality. Therefore, module dependency issues are not unusual things when it comes to cross-platform engineering. So I just want to mention about what I experienced when I use React Native. If some external modules that I need are depending on different versions of React Native or React, I alternatively had to choose lower version of the module. (For me, upgrading React Native / React version was more risky because it can generate other conflicts from somewhere else.) Even though you finally find some working sets of them, still should check if there is any conflict on another platform. After install required modules, it needs to be linked to native(Android and iOS). React Native provides useful command line tool ‘react-native link’ to add dependency, but sometimes you have to designate module path manually for both platform in separated way. Therefore, you will be required to know more deeply about native platform than you expected. This is something you can experience if your app gets bigger and relies on various modules. It’s too early to talk about this issue in Flutter, but so far I didn’t have to go through all these kinds of procedures in Flutter yet, just configuring package spec file fixed some version mismatch issues.

<br>

### Built-in Components (Widgets)
&nbsp;Both React Native and Flutter offers many useful built-in UI components(*It's called widget in Flutter*). In React Native, there are basic components and some platform specific components for iOS and Android. In this way, some native components may not work properly on other platform so you need to selectively use them. One thing I'm still uncomfortable about React Native is Navigation. It's not a built-in and officially suggests external modules. From my experience, navigation modules commonly used are not so handy to implement even though navigation is one of the most essential factors for mobile application.

In Flutter, it also offers plenty of rich built-in widgets. Essentially it is all about 'Widget' and the range of built-in widgets is really wide. For example, it gives Cupertino theme for iOS and Material for Android. Using these Theme widget, you can just grab pretty much every UI element for each platform without thinking about tedious style numbering and looking for measurement specs. Such as Hero and SliverWidget are already embedded with rich animation effects. 'Hero' widget is actually what I used for the demo application above.

&nbsp;Another widget that I found out useful in Flutter is 'RichText' widget. In React Native, if I need to change some part of style of characters within paragraph, I had to translate it as html format and then able to apply different styles. But using 'RichText', you can put multiple TextSpan child widgets inside of it and give various styles on each of them.

&nbsp;Although these conveniences, depending on widgets which already has lots of functionlities itself such as 'Hero', makes the app difficult to customize in personal taste.

<br>

### Styling
&nbsp; In React Native, it uses JSX syntax just like HTML but with React component. And for the style of each component, it uses StyleSheet and it has really similar to CSS. Here I wrote some code to render simple text components.
```
// index.js
<View style={styles.container}>
  <Text style={styles.textOne}>Text 1</Text>
  <Text style={styles.textTwo}>Text 2</Text>
  <Text style={styles.textThree}>Text 3</Text>
</View>

// styles.js
StyleSheet.create({
  container: {
    flex: 1,
    width: '100%',
    paddingHorizontal: 8,
    paddingVertical: 16,
    alignItems: 'flex-start',
    borderRadius: 10,
    borderWidth: 1,
    borderColor: '#fff'
  },
  textOne: {
    fontSize: 12,
    color: 'white',
    padding: 10,
  },
  textTwo: {
    fontSize: 22,
    color: '#333333',
    padding: 5
  },
  textThree: {
    fontSize: 14,
    color: 'black',
    padding: 8
  }
})
``` 
&nbsp;In Flutter by the way, it doesn't have separate style sheet. Inside build() function, it's required to return Widget to render view. Such as padding, Margin, Alignment etc. all of style components come as a Widget. You wrap widgets with another widgets and so on, and it's called 'Widget Tree'. 

&nbsp;I added a code snippet below to help you understand how it's different compare to React Native. As above RN code, it also renders 3 Text widgets in a column with each text style inside decorated box container.
```
Container(
    decoration: BoxDecoration(
      borderRadius: BorderRadius.only(
          bottomLeft: Radius.circular(10.0),
          bottomRight: Radius.circular(10.0)
      ),
    ),
    width: double.infinity,
    padding: const EdgeInsets.symmetric(horizontal: 8.0, vertical: 16.0),
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: Text(
            "Text 1",
            style: TextStyle(
                color: Colors.white,
                fontSize: 12.0
            ),
          ),
        ),
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: Text(
            "Text 2",
            style: TextStyle(
                color: Colors.yellow,
                fontWeight: FontWeight.bold,
                fontSize: 22.0
            ),
          ),
        ),
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: Text(
            "Text 3",
            style: TextStyle(
                color: Colors.white.withOpacity(0.7),
                fontSize: 14.0
            ),
          ),
        )
      ],
    ),
  )
```
&nbsp;Padding is a widget able to contain single child widget to pass its padding property. Column is a widget extending Flex widget and it gives vertical alignment to its multiple child widgets.

&nbsp;I tried to make a similar amount of view using both frameworks. Flutter code seems longer and more verbose than React Native, but it really depends on your preference. If you are a web developer, React Native way would much easier for you. From my case, I was originally an android developer and familiar to separating styling part from rendering one. But actually it didn't take long time to adjust Flutter syntax. As it shown, it's intuitive. You can expect what style is combined to which widget even though you don't know about Flutter at all. Personally I felt that the widget tree is quite convenient especially when tweaking UI using hot-reload because you can see how the view is constructed with styles in a single view and also the changes that you made.

<br>

### Conclusion
&nbsp;I used React Native as a mobile development tool for quite a long time and could see lots of difficulties as well as benefits. From what I learned from Flutter, various built-in widgets provided are easy to implement but could be less customizable and the application can be too much dependent on Flutter. Every tool comes with pros and cons, so I strongly recommend you to actually try them out. 
For now I'm just using Flutter for learning purposes, but when it comes to real project, all the major issues of that tool will appear up once the product gets bigger. Especially cross platform framework like Flutter or React Native, essentially they are trying best to do act like native not native. So they may not show you the best performance as much as native but will give you flexible accessibility between different platforms and "code-reuse" benefits. Therefore, when you are considering to use cross platform framework rather than native, look into it carefully whether the key functionalities and the goal of your product can be satisfied enough with the framework or not. 



