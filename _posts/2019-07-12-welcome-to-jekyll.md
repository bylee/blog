---
layout: single
title:  "블로그 시작하기"
date:   2019-07-12 13:56:21 +0900
categories: blog
tags: life jekyll
---
# 글을 어디에 쓸까?
처음에는 미디엄이나 브런치에 쓰려고 했는데, 생각보다 자유도가 떨어진다는 것을 알고나서 아예 정적 페이지 생성기쪽으로 생각을 바꿜다. 요즘 뜨고 있는 게츠비를 고려했는데, 커스터마이징이나 문제 해결에 시간을 쓰다보면 주객인 전도될 것같아 지킬로 작성해서 깃헙에 올리기로 하였다.

## 지킬이란?
간단한 명령으로 정적 사이트를 생성할 수 있는 오픈 소스이다. 간단한 사용법과 많은 테마, 쉬운 커스터마이징으로 많은 인기를 끌고 있다. 나같은 경우는 사이트의 모양이나 기능을 크게 따지지 않고 무난하게 쓸 수 있는 것을 선택하였다.

지킬 사이트에 들어가 보면 간단한 명령어 몇 개로 사이트를 생성할 수 있음을 알 수 있다.

![Jekyll site](/assets/images/welcome-to-jekyll/intro-jekyll.png "Jekyll site")

그런데, 첫번째 명령은 루비의 gem을 사용하는 것이다. 우선 루비를 설치해야겠군.

# 루비 설치하기

루비 설치를 위해 [https://rubyinstaller.org][rubyinstaller]{:target="_blank"}에 들어가보면, 큼지막하게 Download라는 버튼을 볼 수 있다. 눌러보자.

![Install ruby #1](/assets/images/welcome-to-jekyll/install-ruby01.png "Install ruby #1")

다운로드를 위한 링크들이 보이고, Ruby+Devkit 2.5.X (x64) 를 추천한다는 문구가 보인다. 왼쪽에 추천하는 버젼의 링크가 눈에 띄인다.

![Install ruby #2](/assets/images/welcome-to-jekyll/install-ruby02.png "Install ruby #2")

다운로드 후에 설치 프로그램을 실행해보자. 사용 동의를 요구하는 화면이 나타나면 동의를 선택하고, Next 버튼을 누른다.

![Install ruby #3](/assets/images/welcome-to-jekyll/install-ruby03.png "Install ruby #3")

이후에 계속 Next 버튼을 누르면, 설치가 성공했다는 메시지와 함께 툴체인 설치를 할지 물어본다. 툴체인은 루비가 아닌 c나 c++를 빌드하는데 필요한 프로그램이라고 이해하면 된다.

![Install ruby #4](/assets/images/welcome-to-jekyll/install-ruby04.png "Install ruby#4")

나는 나중을 위해 msys2와 mingw 툴체인 모두 설치하였다. 필요에 따라 선택하면 된다.
![Install ruby #5](/assets/images/welcome-to-jekyll/install-ruby05.png "Install ruby#5")

# Jekyll 설치하기
이제 지킬을 설치해보자. 홈페이지에 나와있는 것처럼, 다음 명령으로 설치할 수 있다.

{% highlight bash %}
$ gem install bundler jekyll
{% endhighlight %}

지킬 설치가 끝났다면, 지킬로 홈페이지를 생성할 차례이다. 지킬로 생성할 사이트의 이름은 bylee.github.io 인데, github에 호스팅하기 위한 이름이다.

{% highlight bash %}
$ jekyll new bylee.github.io
{% endhighlight %}

![Generate site using Jekyll](/assets/images/welcome-to-jekyll/install-jekyll01.png "Generate site using Jekyll")

이것으로 사이트 생성까지 끝났다.

# 깃헙에 게시하기
생성된 사이트를 깃헙에 게시하려면 간단히 다음 명령으로 가능하다.

{% highlight bash %}
$ cd bylee.github.io
$ git init
$ git add .
$ git commit -m "Initial commit"
$ git remote add origin git@github.com:bylee/bylee.github.io.git
$ git push -u origin master
{% endhighlight %}

정상적으로 깃헙에 올라갔다면 브라우져로 [https://bylee.github.io][myblog]{:target="_blank"}으로 접속해보자

![Published blog](/assets/images/welcome-to-jekyll/install-jekyll02.png "Published blog")

정상적으로 게시된 블로그를 볼 수 있다.

[rubyinstaller]: https://rubyinstaller.org
[myblog]: https://bylee.github.io
