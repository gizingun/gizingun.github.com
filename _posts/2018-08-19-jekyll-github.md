---
layout: post
title: jekyll과 github로 블로그 시작하기
description: "jekyll을 통해 블로그를 만들고 github page에 호스팅하는 방법을 설명"
tags: [blog, gitgub, jekyll]
---

## 과정 요약

블로그 설치 과정은 다음과 같다. 터미널을 통해 jekyll을 설치하고, github에 `blog`라는 이름으로 repository를 만들고 블로그 내용을 올리게 될 것이다. 생성된 repository 주소는 `https://github.com/사용자명/blog.git`이 되고, 이 과정에서 생성되는 블로그의 주소는 `http://사용자명.github.io/blog`가 된다.

## 터미널을 통한 jekyll 설치

OS X에서 터미널을 이용해 jekyll 툴을 원활히 사용하기 위해서는 사전에 Xcode와 커멘트라인 툴이 설치되어 있어야 한다. jekyll는 다음의 명령으로 설치한다.

	[sudoe] gem install jekyll

## 프로젝트 주소 페이지 방식으로 블로그 생성

	jekyll new blog
	cd blog
	jekyll serve --watch

`jekyll new blog`현재 디록토리 밑에 blog 디렉토리를 만들고 jeckyll 기본 디렉토리와 파일을 생성한다. jeckyll은 기본적으로 서버를 내장하고 있어 NgineX와 같은 별도의 웹서버를 띄우지 않아도 만들어진 사이트를 확인할 수 있다. `jekyll serve --watch`를 실행하면 `localhost:4000`에서 로컬에 생성된 디폴트 jekyll 페이지를 볼 수 있다.

`--watch` 옵션은 사이트를 변경하는 대로 다시 빌드하여 브라우저에서 변경사항을 바로 확인할 수 있게하는 옵션이다.

	git init
	git checkout --orphan gh-pages
	git remote add origin 저장소url
	git add .
	git commit -m "Initialize blog";
	git push origin gh-pages

이제 `사용자명.github.io/blog`의 주소로 이동하면 자신의 블로그를 볼 수 있다. 참고로 동기화과정이 조금 오래 걸릴 수 있다.

## 블로그에 포스팅하기
jekyll은 각 블로그 글마다 파일이 따로 구성되어있다. 즉, jekyll이 각각의 파일을 보여주는 것이다. 그러므로 글을 등록하기 위해서는 로컬 디렉토리에 포스팅하고자 할 내용을 파일로 저장하고 `_post`폴더에 저장해야 한다. jekyll은 `_post`폴더에 있는 파일만 블로그 포스트로 인식한다.

블로그 포스트 파일명은 `YYYY-MM-DD-post-name.확장자` 형식으로 지정되어야 한다. 마크다운으로 작성된 경우 `YYYY-MM-DD-포스트이름.md`로 저장하면 된다. 또한 블로그 포스트는 항상 파일 앞에 `front matter`를 적어야 한다. jekyll는 front matter 블록으로 시작되는 파일만 처리한다. 각 front matter에는 **title**과 **layout** 필드는 반드시 들어가야 한다.

	---
	title: [제목]
	layout: post
	---

	글제목

이처럼 jekyll를 설치한 후 블로그를 github page에 무료로 호스팅하는 방법을 알아보았다.

## 참고

* [지킬 공식 사이트 한글 번역](http://svperstarz.github.io/jekyll-docs-ko/)
* [지킬로 깃허브에 무료로 블로그 만들기](https://nolboo.github.io/blog/2013/10/15/free-blog-with-github-jekyll/)