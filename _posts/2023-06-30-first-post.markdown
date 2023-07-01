---
layout: post
title:  "How to make my Page"
date:   2023-06-30 22:03:36 +0530
categories: jekyll
---

*GitHub Pages를 이용하여 개발 블로그 만들기 성공!*

저장할 폴더를 생성하여 그곳에 나의 repository를 clone해온다.

repository 만드는 법은 구글링하면 쉽게 생성할 수 있으므로 생략

***ehyeong.github.io*** <- my repository

clone한 장소로 가서 인덱스 파일 하나를 생성한다.

echo "Hello World" > index.html

git add *

git commit -m "Start git blog"

git push -u origin main

이제 내 페이지를 들어가면 "Hello World" 인덱스 파일이 모이는 것을 확인할 수 있다. 

이제 문제는 jekyll 설치!

~~나머지는 알바 다녀와서 쓰도록 하겠다 ^^~~

***

구글링을 통해 인터넷에 나와있는 대부분의 방법으로 시도해본 것 같다. 그렇지만 뭐가 문젠지 쉽게 설치가 되지 않았다.

결론적으로 [jekyll설치](https://youtu.be/UKB9ylw0G4U,  "youtube link") 영상을 참고하여 해결하였다. 

jekyll 설치가 되고 bundle exec jekyll serve 을 통해 http://127.0.0.1:4000/ 에 접속하여 나의 페이지가 생성된 것은 확인하였으나, GitHub에 올려도 그 페이지를 확인하긴 어려웠다.

~~다음날 맑은 정신으로 다시 시도하기로 하고 종료~~

***

gem install bundler

gem install jekyll

rm -f index.html (인덱스 파일 삭제)

jekyll new ./

!오류 jekyll 4.3.2 | Error:  Operation not permitted

하라는대로 했는데 오류가 난다.

다시!

rbenv versions 

내 버전을 확인해보니 3.0.6이다 (문제없는듯)

gem install bundler

rbenv rehash

cd ehyeong.github.io

gem install jekyll  <- github.io 안에 설치해야하는 것인가? 나도 모르겠다.

jekyll new ./

!conflict 오류

jekyll new ./ -f <- 강제로 하겠다는 뜻인가?

어쨌든 하면 New jekyll site installed in /Users/hyeonji/ehyeong/ehyeong.github.io. 설치 성공

bundle install 

bundle exec jekyll serve

하고 git에 올리면 드!디!어! 내 사이트 https://ehyeong.github.io 에 들어가면 방금 생성한 페이지로 이동이 가능하다 

하지만 지금은 빈 페이지이다.

~~그렇다면 꾸며야겠지 ? ( ͡° ͜ʖ ͡°)~~

***

나는 이 [테마](github.com/thelehhman/plainwhite-jekyll, "github link")를 사용하였다.

다운받아서 나의 github.io 폴더에 옮기고 실행시켰더니 테마가 적용되었다.

_config.yml 파일을 열어 내용 및 이미지를 수정해 주었다.

그 다음 문제는 포스팅 하는 법 이다.

_posts 폴더안에 markdown 문법으로 md파일을 생성하면 글을 올릴 수 있다고 하였다.

그런데... 그런데...

나는 md파일을 작성하여 git에 올려도 아무런 변화가 없었다. 
*whyrano* ｡ﾟ(ﾟ´Д｀ﾟ)ﾟ｡

그치만 몇번의 노력끝에 나 이현지 성공했다.

그러니 이렇게 글을 쓰고 있단 말씀 ! ʅ（◞‿◟）ʃ

하나하나 구글링해서 해보라는 대로 수정해가며 

commit -m "post first try"

commit -m "post second try"

commit -m "post third try"

...

끝에 나의 first post가 올라가는 것을 확인할 수 있었다.

내 생각에 날짜를 하루 전으로 작성해야 하는 것 같다.

나는 글 작성을 7/1에 하고있으나 6/30일로 입력해야 올라갔다. ~~왜지~~

***
참고 링크

[마크다운 문법](https://gist.github.com/ihoneymon/652be052a0727ad59601, "github link")

[블로그 생성1](https://zeddios.tistory.com/1222, "blog link")

[블로그 생성2](https://supermemi.tistory.com/entry/%EB%82%98%EB%A7%8C%EC%9D%98-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-Git-hub-blog-GitHubio, "blog link")

***

많은 오류들과 싸우느라 힘들었는데 막상 작성하니 별게 없어보인다.
뒤죽박죽에 가독성도 떨어지고 아직 많은 문제들이 있으나 차차 해결해나가기로 하자!

앞으로 여기에 개발하는데 있어서 모든 것을 정리할 예정이다.

만관부(승관) ☆〜（ゝ。∂）