---
layout: post
title: "React Native and Flutter"
cateogiries: []
tags: [flutter,react-native,cross platform framework, mobile development]
author: Dooyoung Gi
---


<p>&nbsp;When I first learned about React Native two years ago, I found it fascinating that I could develop iOS and Android applications using the same codebase. I was especially impressed by the especially hot-reloading functionalities, which accelerated UI development cycle.
</p>
<p>
&nbsp;Recently, I got a chance to learn another cross-platform framework, Flutter, which is developed by Google. I made a simple demo application, one requiring fairly tricky animation effects. After a few months of playing around on Flutter, I thought I’d share my experience with it and how it compares to React Native. To be fair, I still have lots of things to try out in Flutter, so this isn’t a comprehensive comparison, but I thought it could still be useful for someone who is interested in these platforms.
</p>
<p>
&nbsp;I created a mobile app that offers rewards and deals from local businesses. The main goal of this demo was to implement the following features:</p>

* Horizontal scroll view to show top deals available.
* Capture and dropping animation effects for ‘add to favorites’
* Vertical drawer to see favorited items.
* Hero(shared elements) animation to navigate to details screen.

<img 
  src="/assets/reward_demo.gif" 
  width="270" 
  height="560" />
<br>
[See how our our visual designer worked with our developer in Flutter to create this app](https://medium.com/fm-stories/how-developers-and-designers-can-collaborate-using-flutter-35b3e49046ca)

<p>
&nbsp;Flutter uses Dart, which was a new language to me. But I was able to implement the features above in just a few days using Flutter — a pleasant surprise.
</p>
<br>
  
### How React Native and Flutter do they work behind the scenes

| Framework    | Language     | Environment                              |
|:-------------|:-------------|:-----------------------------------------|
| Flutter      | Dart         | DartVM                                   |
| React Native | JavaScript   | JavaScriptVM(JavaScriptCore, Chrome V8)  |

<p>
&nbsp;So, how are React Native and Flutter different? Here’s a look at the overall process of each framework.
</p>
<p>&nbsp;Flutter uses Dart and runs on DartVM. Basically DartVM has two ways to execute Dart code. One is JIT (“Just in Time”) and the other one is AOT (“Ahead of Time”) compiled snapshots. In debug mode, Flutter creates a persistent process which uses state caches to re-compile only the part of the code that has changed, and this comes to as hot-reload in Dart JIT. In profile and release mode, Flutter uses app AOT snapshots, which shows faster start-up time and best performance without warm-up time.

&nbsp;React Native uses JavaScript and runs on the Chrome V8 engine for debugging mode. Since JavaScript is a dynamically typed language, it’s best for utilizing JIT but not applicable to AOT. So, React Native uses JavaScriptCore to interpret JavaScript for iOS. React Native will create the bundle with our JavaScript code and send it to the device. Then, it will render the native UI by sending commands to the native platform via its bridge, which serves as an interface between JavaScript and native modules of our React Native application as appropriate (Objective-C for iOS or Java for Android).
</p>

<br>

### Built-in Components (Widgets)
&nbsp;Both React Native and Flutter offers many useful built-in UI components. In Flutter, they’re called widgets. In React Native, there are basic components and some platform-specific components for iOS and Android. In this way, some native components may not work properly on other platforms, so you need to use them selectively.

I find the navigation in React Native a bit tricky. It doesn’t have a built-in tool and requires bringing in external modules. This takes time and can be hard to manage.

Flutter, on the other hand, offers plenty of rich built-in widgets. The range of widgets is wide. For example, it provides Cupertino theme for iOS and Material for Android. Using these Theme widgets, you can just grab pretty much every UI element for each platform without thinking about tedious style numbering and looking for measurement specs. Hero and Sliver Widget are already embedded with rich animation effects. Hero is actually what I used for the demo application above.


&nbsp;Another widget that I found to be useful in Flutter is the RichText widget. In React Native, if I needed to change some aspect of character style within a paragraph, I had to translate it as HTML format in order to apply different styles. But using RichText, I can put multiple TextSpan child widgets inside and and apply various styles to each of them.

&nbsp;Although these widgets are very convenient, do note that it can be difficult to customize your app deeply.

<br>

### Styling
&nbsp; React Native uses JSX syntax just like HTML but with React components. And for the style of each component, it uses StyleSheet which is similar to CSS. Here I wrote some code to render simple text components.
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
&nbsp;Flutter, by the way, doesn't have a separate style sheet. The build() function is responsible for returning the widget to render the view. Padding, margin, alignment and other style components come as a widget. You wrap a widget with another widget and so on, creating a widget tree.
&nbsp;I added a code snippet below to help you understand how Flutter is different from React Native. In the above React Native code, it renders 3 text widgets in a column with each text style inside a decorated box container.
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
&nbsp;Padding is a widget containing a padding property and a child widget, to which that padding applies. Column is a widget that extends Flex widget and it provides vertical alignment to its multiple child widgets.

&nbsp;I tried to make a similar view using both frameworks. Flutter code seems longer and more verbose than React Native, but it really depends on your preference. If you are a web developer, React Native could be easier for you. In my case, I was originally an android developer and familiar with separating styling from rendering. But actually it didn't take me long to adjust to Flutter syntax. It felt intuitive. I found the widget tree quite convenient, especially when tweaking UI using hot-reload because I could you can see how the view is constructed with styles in a single view and also the changes that I had made.

<br>

### Module Dependencies
&nbsp;It will be not enough to only use built-in components to implement all parts of your app. You will also need external modules to extend functionality. (These are called ‘plugins’ in Flutter). Therefore, module dependency issues could occur, which is not unusual when it comes to cross-platform engineering. 
<br>
&nbsp;Let me explain what I experienced when I use React Native: If some of the external modules that I needed depended on different versions of React Native or React, I had to choose the lowest common denominator versions of the module. (Upgrading the version of React Native and React was more risky because that could generate other conflicts from somewhere else.) After finding modules that work together, I still needed to check to see if there were any conflicts on other platforms. The modules also needed to be linked to the native platform. React Native provides useful command line tool ‘react-native link’ to add dependency, but sometimes I needed to designate the module path manually for both platforms, requiring me to understand the native platforms more than I expected. The bigger your app gets, the more it relies on various modules, and the more likely you’ll encounter this scenario.
<br>
&nbsp;While I haven’t had enough experience in Flutter to see if there’s a similar issue, so far I haven’t encountered it. Configuring package spec files fixed some version mismatch issues that came up. 


<br>

### Conclusion
&nbsp;I used React Native as a mobile development tool for quite a long time and could see lots of difficulties as well as benefits. From what I learned from Flutter, various built-in widgets provided are easy to implement but could be less customizable and the application can be too much dependent on Flutter. Every tool comes with pros and cons, again, it really depends on your personal taste. So I strongly recommend you to actually try them out.
<br>
&nbsp;For now I'm just using Flutter for learning purposes, but when it comes to real project, all the major issues of that tool will arise once the product gets bigger. Especially cross platform framework like Flutter or React Native, essentially they are trying best to behave like native apps when they are not. So they may not show you the best performance as much as native apps, but will give you flexibilty between different platforms followed by "code-reuse" benefits. Therefore, when you are considering to choose a cross-platform framework rather than native, look into it carefully whether the key functionalities and the goal of your product can be satisfied enough with the framework or not.




