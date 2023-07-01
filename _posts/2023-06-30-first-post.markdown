---
layout: post
title:  "How to make my Page"
date:   2023-06-30 22:03:36 +0530
categories: Blog
---

### GitHub Pages를 이용하여 개발 블로그 만들기 성공!

저장할 폴더를 생성하여 그곳에 나의 repository를 clone해온다.

repository 만드는 법은 구글링하면 쉽게 생성할 수 있으므로 생략

ehyeong.github.io <- my repository

clone한 장소로 가서 인덱스 파일 하나를 생성한다.

echo "Hello World" > index.html

git add *

git commit -m "Start git blog"

git push -u origin main

이제 내 페이지를 들어가면 "Hello World" 인덱스 파일이 모이는 것을 확인할 수 있다. 

이제 문제는 jekyll 설치!

여기부터는 알바를 다녀와서 쓰도록 하겠다 ^^