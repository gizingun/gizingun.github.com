---
layout: post
title: jekyll과 github로 블로그 시작하기
description: "jekyll을 통해 블로그를 만들고 github page에 호스팅하는 방법을 설명"
tags: [blog, gitgub, jekyll]
---

github.io로 잘 알려진 GitHub Pages는 개발자들 사이에서 개인 블로그로 인기가 높습니다. 다른 사람들의 잘 꾸며진 github 페이지를 보면서 나도 공부한 내용이나 새로 알게된 내용을 정리해야지라고만 생각했다가 이번에 Jekyll을 통해 쉽게 블로깅을 할 수 있다는 것을 알고 정리해보려고 합니다.

## Why Jekyll

Jekyll이 무엇이길래 블로그를 만드는데 Jekyll을 사용하는 것일까요? 간단히 설명하면 Jekyll은 여러 다양한 형태의 텍스트와 테마를 소스로 하여 정적 HTML 웹사이트를 만들어주는 역할을 하는 도구입니다. 예를들면 사용자가 Markdown으로 문서를 작성하면 Jekyll은 이를 HTML파일로 변환하여 우리에게 익숙한 웹페이지의 형태로 보여질 수 있게 합니다. 이를 통해 사용자는 HTML을 신경쓰지 않고 Markdown 양식로 컨텐츠의 생성에 집중할 수 있습니다.

## Why GitHub Pages

우리가 작성한 문서를 다른 사람이 찾아보거나 웹상에 공개하려면 호스팅 서비스를 이용해야합니다. 이때 GitHub Pages를 통해 무료로 호스팅을 할 수 있고 게다가 GitHub Pages는 Jekyll을 자체 지원하기 때문입니다. 즉, 사용자는 GitHub Pages에 Markdown 문서를 올리면 이 문서를 자동으로 변환하여 블로그를 자동적으로 업데이트 할 수 있게 됩니다. 이처럼 GitHub Pages를 이용하면 별도의 설치나 설정없이 공개되어 있는 Jekyll테마를 이용하여 수준급의 블로그를 시작할 수 있습니다.
또한 GitHub 자체가 Git을 위한 온라인 저장소를 제공하기 때문에 블로그에 대한 버전 관리 및 백업도 쉽게 할 수 있습니다.

## 저장소 만들기

GiyHub에서 새 Repository를 만듭니다. 이때 주의할 점은 반드시 저장소의 이름을 GitHub의 username뒤에 `.github.io`가 붙은 이름으로 짓습니다. 이렇게 해야만 `[username].github.io`의 도메인을 가지는 블로그가 됩니다.

## 마음에 드는 Jekyll 테마 찾기

Github에는 이미 [수 많은 Jekyll 테마](https://github.com/topics/jekyll-theme) 들이 공개되어 있습니다. 이들 중에서 선택하여 내 블로그 저장소에 복사해도 되고 아니면 [지킬테마들만 모아놓은 곳](http://jekyllthemes.org)에서 선택하여 사용해도 좋습니다.

## Jekyll 테마 적용

선택한 Jekyll Theme 파일을 내 저장소에 저장한 다음 `_config.yml` 파일의 내용중 다음을 수정합니다.

```yaml
url                      : "https://[username].github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "" # the subpath of your site, e.g. "/blog"
```
저장소의 `_config.yml`을 수정한 후 변경된 내용을 원격 저장소에 push한다면 잠시후 `[username].github.io` url을 통해 선택한 Jekyll테마가 적용된 블로그를 확인 할 수 있습니다.


## 블로그에 포스팅하기

Jekyll은 각 블로그 글마다 파일을 따로 구성합니다. 즉, Jekyll은 각각의 작성된 파일을 각 포스트(게시글)로 보여주는 것입니다. 그러므로 글을 등록하기 위해서는 저장소의 `_post`폴더에 포스팅하고자 할 내용을 파일로 저장해야 합니다. 이때, Jekyll은 `_post`폴더에 있는 파일만 블로그 포스트로 인식합니다.

블로그 포스트 파일명은 `YYYY-MM-DD-post-name.확장자` 형식으로 지정되어야 하며, Markdown으로 작성된 경우 `YYYY-MM-DD-포스트이름.md`로 저장하면 됩니다. 또한 블로그 포스트는 항상 파일 앞에 `front matter`를 적어야 합니다. jekyll는 front matter 블록으로 시작되는 파일만 처리하고, 각 front matter에는 **title**과 **layout** 필드는 반드시 들어가야 합니다.

	---
	title: [제목]
	layout: post
	---

	글제목

## 참고

* [지킬 공식 사이트 한글 번역](http://svperstarz.github.io/jekyll-docs-ko/)
* [지킬로 깃허브에 무료로 블로그 만들기](https://nolboo.github.io/blog/2013/10/15/free-blog-with-github-jekyll/)
* [쉽고 빠르게 수준급의 GitHub 블로그 만들기 - Jekyll Remote Theme으로](https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html)