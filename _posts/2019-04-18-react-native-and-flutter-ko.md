---
layout: post
title: "React Native and Flutter"
cateogiries: []
tags: [flutter,react-native,cross platform framework, mobile development]
author: Dooyoung Gi
---


<p>&nbsp;2년 전 처음 리액트 네이티브를 접했을 때는, 같은 코드베이스로 안드로이드와 iOS앱 개발을 동시에 할수 있다는 것에 흥미로웠다. 특히 hot-reloading과 같은 기능은 UI개발 사이클을 상당히 단축시킨다 점이 아주 마음에 들었다.
</p>
<p>
&nbsp; 최근에 또다른 크로스 플랫폼 프레임워크를 배워볼 수 있는 기회가 생겼는데, 구글에서 개발한 Flutter이다. 지금까지 약 3개월정도 Flutter를 가지고 놀았는데, 그간 나의 리액트 네이티브 경험과 Flutter를 비교해보면 어떨까 싶었다. 아직 Flutter에서 시도해보지 않은 것들이 많아 공평한 비교가 되지는 못하겠지만, 혹시나 나중에 이러한 플랫폼들에 관심이 있는 분들이 이 글을 읽고 조금이나마 도움이 됬으면 하는 바람이다.
</p>

### 데모 프로젝트
<p>
&nbsp;이전에 우리 팀이 만든 디자인을 이용해서 Flutter를 사용해보기로 했다. 이 프로토타입은 개발자와 디자이너간의 Flutter를 이용한 협업을 보여주기위해 제작된 간단한 리워드 모바일 앱이다. 나는 이 디자인에 기능을 추가해 데모 어플리케이션을 제작해보았다. 이 데모를 통해 아래의 기능들을 구현하고자 했다:
</p>

* 이용가능한 딜을 보여주기 위한 가로형 스크롤 뷰
* 캡처 와 드롭핑 애니메이션 효과
* 즐겨찾기 목록을 보여주기 위한 하단 서랍 메뉴 뷰
* 디테일 스크린으로 전환하기 위한 Hero 애니메이션 효과(이미지 공유)

<img 
  src="/assets/reward_demo.gif" 
  width="270" 
  height="560" />
<br>

<br>
  
### 리액트 네이티브와 플러터가 동작하는 방식

| Framework    | Language     | Environment                              |
|:-------------|:-------------|:-----------------------------------------|
| Flutter      | Dart         | DartVM                                   |
| React Native | JavaScript   | JavaScriptVM(JavaScriptCore, Chrome V8)  |

<p>
&nbsp;그렇다면, 리액트 네이티브와 플러터가 어떻게 다르게 동작할까? 각각의 프레임워크가 전반적으로 어떻게 동작하는지 알아보았다.
</p>
<p>&nbsp;플러터는 Dart언어를 사용하고 DartVM에서 동작한다. 기본적으로 DartVM은 두가지 방식으로 Dart코드를 실행하는데, 하나는 JIT("Just in Time") 방식 이고, 다른 하나는 AOT("Ahead of Time") 컴파일 스냅샷 방식이다. 디버그 모드에서 플러터는 상태를 캐싱해서 변경된 부분만 재컴파일한다. 그리고 이것이 Dart JIT에서 hot-reload가 동작하는 방법이 된다. 프로파일이나 릴리즈 모드에서는 AOT스냅샷 방식을 이용하는데 이는 빠른 스타트업과 웜업(warm-up)이 없이 높은 퍼포먼스를 보여준다.

&nbsp;리액트 네이티브는 자바스크립트 언어를 사용하고 ...(수정중)

</p>

<br>

### 빌트인 컴포넌트 (위젯)
&nbsp; 리액트 네이티브와 플러터 둘다 많은 빌트인 컴포넌트를 제공한다. (플러터에서는 위젯이라고 부른다.) 리액트 네이티브에는 기본 컴포넌트들과 몇몇 중에는 특정 플랫폼에서만 올바르게 동작하는 것들도 있다. 그러므로 iOS나 안드로이드에 적합한 컴포넌트를 선택적으로 사용해야 한다. 그 중 리액트 네이티브에서 내가 가장 다루기 어려웠던 부분이 네비게이션이다. 리액트 네이티브는 네비게이터를 빌트인으로 갖고있지 않고, 사용자는 외부 모듈을 가져와서 사용해야 한다. 이는 시간이 걸리고 관리하기가 쉽지 않다는 단점이 있다. 반면 플러터에서는 굉장히 많은 빌트인 위젯들을 제공한다. 그 범위가 상당히 넓은데, 예를들어 플러터는 iOS를 위한 Cupertino 테마, 안드로이드를 위한 Material테마를 각각 제공하여 개발자들에게 플랫폼 별 색이나 텍스트, 크기 스펙을 찾을 필요가 없도록 도와준다. 또한 Hero나 Sliver위젯의 경우 이 자체가 지닌 애니메이션 효과로도 충분히 UI구현이 가능하기 때문에 애니메이션 구현 시간을 덜어준다. Hero 위젯의 경우 위의 데모 어플리케이션에서 실제로 사용했다.

&nbsp; 플러터에서 발견한 유용하다고 생각한 또다른 위젯중에 RichText가 있다. 리액트 네이티브에서는 한 문장 안에서 글자나 단어 단위의 스타일을 변경하고 싶을 때 HTML형식으로 파싱하여 태그로 스타일을 줄 수 밖에 없었다. 하지만 RichText를 이용하면 그 안에 다수의 TextSpan 위젯을 넣을 수 있고 각각의 TextSpan에 다른 스타일을 적용시킬 수 있다.  
 
&nbsp; 플러터에서 제공하는 이러한 빌트인 위젯들은 사용하기가 굉장히 편리하다. 하지만 역시나 이러한 빌트인 위젯들에 많이 의존하게 되면 차후 UI가 변경되거나 다른 기능들이 추가로 필요할 경우 커스터마이징이 어려울 수 있다는 단점이 있다. 

<br>

### 스타일링
&nbsp; 리액트 네이티브는 HTML과 비슷한 React 컴포넌트 JSX 문법을 사용한다. 그리고 컴포넌트의 스타일 속성은 css와 유사한 StyleSheet을 통해 부여한다. 아래의 코드는 리액트 네이티브에서 간단한 텍스트 컴포넌트를 렌더링 할때 쓰이는 스타일 적용 방식을 보여준다. 
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
&nbsp; 플러터에서 스타일링 방식은 위젯을 통해 이루어진다. build()함수는 위젯을 리턴해서 뷰를 렌더링한다. padding, margin, alignment나 다른 스타일 속성값들이 위젯으로 정의되어 있다. 스타일을 적용하고싶은 위젯을 스타일 위젯으로 감싸는 방식으로 스타일을 부여한다. 아래의 코드를 통해 플러터가 리액트네이티브와 어떻게 다른 방식으로 스타일을 적용시키는지 볼 수 있다.
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
&nbsp;Padding 위젯은 속성값과 함께 그 값을 부여할 child 위젯을 갖고있다. Column 위젯은 수직형 레이아웃과 기본적으로 Flex 위젯을 상속함으로써 flex 속성을 제공하고, 다수의 child 위젯을 가질 수 있다.  

&nbsp;위의 두 코드 모두 비슷한 형태의 텍스트 뷰를 렌더링한다. 플러터의 코드가 리액트 네이티브 방식보다 좀더 장황해 보인다. 하지만 이부분은 지극히 취향차이인데, 만약 웹 개발자라면 리액트 네이티브 방법이 좀 더 친근해 보이겠다. 내 경우에는 안드로이드 개발자였고 스타일과 렌더링 파트는 분리하는것에 익숙했다. 하지만 플러터 방식에 익숙해지는데 그리 오래 걸리지는 않았다. 꽤 직관적이기 때문이다. 이 위젯 트리라는 것이 유용하다고 느꼈을 때가 hot-reload기능을 사용할때 였다. UI 요소를 변경하면 그것이 즉각적으로 나타나고 이 때 뷰가 어떻게 구성되었는지 한눈에 볼 수 있는 점이다.

<br>

### 모듈 의존성
&nbsp;빌트인 기능만으로는 어플리케이션의 모든 기능을 구현하는것은 불가능하다. 그래서 기능을 확장하려면 외부 모듈이 필요한데(Flutter에서는 플러그인이라고 부른다) 이 때 모듈 의존성 이슈는 크로스 플랫폼 엔지니어링에서 흔히 발생 할 수 있는 문제이다. 
<br>
&nbsp;리액트 네이티브를 사용할 당시 많은 문제점을 겪었다: 만약 내가 필요한 모듈들이 각각 다른 버전의 리액트 혹은 리액트네이티브 버전에 의존하고 있다면 강제적으로 해당 모듈의 다운버전을 사용해야하는 경우가 발생했다. (리액트나 리액트네이티브의 버전을 바꾸는것은 다른 모듈들에 영향을 미치기 때문에 왠만하면 지양했다.) 적합한 모듈을 발견한 후에는 그것이 다른 플랫폼(ios 혹은 안드로이드)에서도 정상적으로 동작하는지, 충돌은 없는지 확인해야 한다. 다음으로 이렇게 찾은 모듈은 네이티브에 의존성이 연결되어야 하는데 리액트 네이티브에서는 이를 위해 'react-native link'라는 명령어를 제공한다. 이 커맨드 라인 한번으로 해결되어야 간단히 끝나는 일이지만 보통은 각각의 플랫폼 환경에서 사용자가 직접 모듈 경로를 지정해줘야 하는 경우가 빈번하다. 이러한 점에서 네이티브 플랫폼에 대한 이해가 예상했던 것보다 더 필요할 수 있음을 사용자는 인지하고 있어야 한다. 어플리케이션의 규모가 커질수록 많은 모듈들에 의존해야 할 것이고 이러한 시나리오를 더 자주 직면하게 되기 때문이다.
<br>
&nbsp;플러터에서는 아직 많은 모듈이 필요로하는 작업을 해보지 않아서 별다른 이슈를 발견할 수는 없었다. 사소한 버전 충돌 이슈가 있었는데 패키지 스팩(pubspec) 파일에서 버전 수정을 통해 해결되었고 따로 경로를 지정하거나 네이티브 환경에 접근해야 하는 경우는 아직까지 없었다.

<br>

### 결론
&nbsp;리액트 네이티브를 모바일개발 도구로 사용한지 꽤 오래 되었고 이것이 주는 장점 뿐만이 아니라 많은 한계점도 알 수 있었다. 플러터에서 전반적으로 느낀것은, 프레임워크가 제공하는 다양한 빌트인 위젯과 쉬운 구현방법, 하지만 그만큼 커스터마이징에 제한적일 수 있다는 점 때문에 어플리케이션이 프레임워크에 너무 의존할 가능성도 보였다. 모든 도구들은 장단점이 있고 다시 말하지만 사실 이건 지극히 개인의 선호도에 따른 문제이기 때문에 직접 사용해보는 것을 적극 추천한다.
<br>
&nbsp;지금으로써는 단순히 공부의 목적으로 플러터를 사용했지만 실제 프로젝트에 적용되었을때 그 규모가 커질 수록 이전에는 몰랐던 주요 이슈들이 발생하기 시작한다. 특히 이러한 리액트 네이티브나 플러터와 같은 크로스 플랫폼 프레임워크는 최대한 효율적으로 네이티브와 비슷한 사용자환경을 주기 위할 뿐 근본적으로 네이티브는 아니다. 그래서 네이티브 만큼의 최적의 퍼포먼스를 보여주지 않을 수 있겠지만, '코드 재사용' 이라는 이점은 개발자에게 멀티 플랫폼에 대해 상당한 유연성을 제공한다. 따라서 차후 네이티브 대신 크로스 플랫폼 프레임워크를 채택할것을 고려중이라면 해당 제품의 목적과 주 기능들이 그 프레임워크를 통해 충분히 구현될 수 있는지 신중하게 알아보고 결정하는것이 바람직하다고 생각한다.




