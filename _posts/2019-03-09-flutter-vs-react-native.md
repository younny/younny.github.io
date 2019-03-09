---
layout: post
title: "Flutter vs React Native"
cateogiries: [programming]
tags: [flutter,react-native,cross platform,mobile development]
author: Dooyoung Gi
---


When I first knew React Native about 2 years ago, it was fascinating that I could develop iOS application using almost same codebase and especially reload/hot-reload functionalities which made improvement on UI development cycle. I used React Native as mobile development tool for couple of projects in real. Things seemed going smoothly at the begining phase, but I found lots of issues and limitations that RN(React Native) has. Navigation, conflicts between two platforms(iOS and Android), harder implementation of some simple widgets, so on.
<br>Recently, I got a chance to learn another cross platform framework Flutter developed by Google. I made simple demo application which requires fairly tricky animation effects and it’s interesting that how I quickly implemented these animations just in few days using Flutter (Even Dart lang was new to me). So I came up with writing about functionalities that these two platforms have and compare them. It might be too early for me to compare both because I’ve learned Flutter only about 2 months now, but I will just put my overall thoughts on that so that it could help some developers or clients thinking to use React Native or Flutter (even any other framework) for their product.

### Development Environment, Language

| Framework    | Language     | type             |
|:-------------|:-------------|:-----------------|
| Flutter      | Dart         | static, dynamic  |
| React Native | JavaScript   | dynamic          |

React Native uses JavaScript, and Flutter uses Dart. Before going further, I needed to learn what and how it works behind before running on native. [Thanks to this article](https://hacks.mozilla.org/2017/02/a-crash-course-in-just-in-time-jit-compilers/)
* Interpreter - Translation happens line by line, quick. But no optimization. Fit to dynamic typed language such as JavaScript.
* Compiler - Go through all compilation steps. Takes more time for optimization but it runs faster at runtime. AOT(Ahead of Time)
* JIT compiler (Just in Time) - Go through all interpretation when it's executed. Optimizing compiler runs when it's called by monitor. Dynamic, but could be de-optimized, slow execution time.

One of the reason why Flutter uses Dart is because Dart supports both AOT and JIT types. In development, it uses JIT so that generates faster development cycle. For production environment, uses AOT so that it could have more stable and better performance when it's running. And Dart is typed language, it can be compiled to native code directly through AOT compilation.
<br>React Native works differently. There is a 'Bridge' which gives interface between JavaScriptVM and native platforms. RN makes JavaScript bundle file, and sends signal to bridge to execute program from the native entry point. JavaScript thread which got this signal, started issuing works to do from its bundle code. Because of the fact that it doesn't communicate directly each other, there comes loading time and it gets longer once the code gets bigger. These few seconds delay means a lot in real development. Such as Hot-reload feature, for me Flutter has much faster and smooth performance than React Native. Especially, RN seems not that friendly on Android. It gets disconnected frequently in some reasons.


### Module Dependencies
Cross platform framework requires external modules (packages in Flutter) to extend its functionality. When I use React Native, this is the hardest part. If each modules are depending different versions of React Native or React, I had to choose lower version of the module. (Upgrading React Native or React version has more risk that brings more conflicts with other modules.) Even I finally found working pairs of them, unfortunately I figure out this module conflicts on another platform. Well, once you finally found one, you got to link it to each platform. Normally it doesn't work simply such as using 'react-native link'. Sometimes you need to desginate module path for both platform in separated way. I can't say Flutter doesn't have this kind of issues at all, but so far I wasn't annoyed yet.


### Built-in Components(Widgets)
Flutter offers plenty of useful widgets. Actually it is all about 'Widget'. For example, it gives Cupertino theme for iOS and Material for Android. Using Theme widget, you can just grab pretty much every UI elements for each platforms without thinking about tedious style numbering and looking for measurements. Animation widgets Such as Hero and SliverWidget give rich effects and handy implementation.

I tried to implement similar animation effect using React Native. I used Animated component with scale and translation transform. In Flutter, I used Hero Widget. 


### Styling
Flutter doesn't have custom style sheet. Padding, Margin, Alignment all of styles come as a Widget. You wrap these widgets with another widgets and it's called 'Widget Tree'.
```
// It renders 3 rows of text widgets with some styling 
// and it's about 50 lines of code.
Container(
                  decoration: BoxDecoration(
                    color: deal.themeColor,
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
                          deal.category.toUpperCase(),
                          style: TextStyle(
                              color: Colors.white,
                              fontSize: 12.0
                          ),
                        ),
                      ),
                      Expanded(
                        child: Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Text(
                            deal.title,
                            style: TextStyle(
                                color: Colors.white,
                                fontWeight: FontWeight.bold,
                                fontSize: 22.0
                            ),
                          ),
                        ),
                      ),
                      Padding(
                        padding: const EdgeInsets.all(8.0),
                        child: Text(
                          '~ ${getDate(deal.exp)}',
                          style: TextStyle(
                              color: Colors.white.withOpacity(0.7),
                              fontSize: 14.0
                          ),
                        ),
                      )
                    ],
                  ),
                ),

```
It is too long to render just a few Text UI. I don't like it. But you're going to be familiar quickly once you start using Flutter (It's all about widget). Actually I felt it convenient especially when using hot-reload. You can expect which style is combined to which widget, eventhough you don't know about Flutter at all. Haven't you ever experienced bad named or unused style made you confused?
One of widgets that I liked in Flutter is 'RichText' widget. In RN, if I need to change some part of style of characters within paragraph, I had to translate it as html format and then able to apply different style. But using 'RichText', you can put multiple TextSpan childs inside of it and give various styles. (In Dart, String class has padLeft() / padRight() methods. You can apply padding style in the String variable directly.)

### Navigation
I didn't like navigation part in React Native. The most common moddule is react-navigation and the official documentation refers this one to use. But actually it is animation effects which mocks navigation behavior. I exactly saw this issue such as double tapping navigates screen twice. (At the time when I use React Native, there was upgrade to v2 but I couldn't try on my project because it has breaking changes and I didn't want to mess it up.) Another suggested module is using native component but it was early stage for production use. Even now I don't see any clear solution about navigation provided by React Native. I tried making simple navigation app using the latest version of react-navigation. It requires me to install another module 'react-native-gesture-handler'. And additional codes into android native. Finally I could see drawer. (One more step, added icon assets, they don't come together.) 
In Flutter, there is Navigator Widget. In its official documentation, "A Widget that manages a set of child widgets a stack discipline". It supports "Stack" only. For example, screens included in drawer navigator have same hierarchy. So 'navigate' between these screens means pararelly. React Native's navigation module has not only push and pop but also 'navigate'. But in Flutter, there seems only push, pop, replace and delete for Navigator actions. Therefore, to implement screen transition between drawer screens, you can give different view per state changes. It sounds like view exchange rather than transition. Maybe Flutter will add more functionalities for Navigation in the future, but not so many features available for now.


### Conclusion
 I've used React Native quite long time and could see many aspects of it. This post seems like becoming my complains about React Native but it doesn't mean that React Native is bad and Flutter is the best alternative of RN or any other framework. Various built-in widgets provided by Flutter are easy to implement but might be less customizable and the application can be too much rely on Flutter. And some of functionalities are not ready yet. For now I'm just using Flutter for learning purpose, but when it comes to real project, all the major issues of that tool will appear up once the product gets bigger. Especially cross platform framework like Flutter or React Native, essentially they are trying to be native not native itself. Therefore my last thought on it, when you are considering to use cross platform framework rather than native, look into it carefully whether the key functionalities and the goal of your product can be satisfied enough with the framework or not. Its "code-reuse" specialty can give you fast development cycle and easy pathway for team. But the worst situation is you become a hybrid one who is fixing total 3 parts, the framework, Android and iOS.


