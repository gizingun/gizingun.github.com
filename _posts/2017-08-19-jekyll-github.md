---
layout: post
title: Jekyll과 Github로 블로그 시작하기
description: "Jekyll을 통해 블로그를 만들고 GitHub Pages에 호스팅하는 방법을 설명"
tags: [blog, gitgub pages, jekyll]
---

github.io로 잘 알려진 GitHub Pages는 개발자들 사이에서 개인 블로그로 인기가 높습니다.
잘 정리된 블로그는 개인에게 만족감도 주지만 그와 더불어 공부한 내용을 정리하는데 또한 블로그만한 것이 없습니다. 블로그는 글을 쓰면서 생각을 정리하는 역할과 더불어 작성한 내용을 온라인 상에 백업하는 역할도 겸하기 때문에 예전부터 블로그를 시작해야지 라고 생각만하고 있었습니다.
하지만 블로그를 운영을 위해서는 여러가지 생각할 것들이 있습니다. 가령 어떤 방법을 통해 블로그를 만들지 어떻게 호스팅을 할지(혹은 어떤 호스팅 서비스를 이용하여 블로그를 시작할지 등) 등의 문제들만 고민하다가 차일피일 미뤄왔었는데 이번에 친구의 조언을 통해 Jekyll 기반의 Github Pages를 사용하여 블로그를 시작하기로 마음먹었습니다. 
이번 post는 Jekyll 과 GitHub Pages에 대한 자세한 내용을 다루기보단 이 둘을 사용하여 첫 블로그를 어떻게 시작했는지를 정리하고자 합니다.


## What is Jekyll


그럼 위에서 언급한 Jekyll 이 무엇이길래 블로그를 만드는데 Jekyll을 사용하는 것일까요?
간단히 설명하면 Jekyll은 여러 다양한 형태의 텍스트(ex. Markdown 형식)와 테마를 소스로 하여 정적 HTML 웹사이트를 만들어주는 역할을 하는 도구입니다.
예를들면 사용자가 Markdown으로 문서를 작성하면 Jekyll은 이를 HTML파일로 변환하여 우리에게 익숙한 웹페이지의 형태로 변환시켜 주는 역할을 합니다. 
즉, 사용자는 HTML을 신경쓰지 않고 Markdown형태의 문서작성만 하면서 블로그의 컨텐츠 생성에만 집중할 수 있습니다.


## Why GitHub Pages


우리가 작성한 문서를 다른 사람이 찾아보거나 웹상에 공개하려면 호스팅 서비스를 이용해야합니다. 
호스팅 서비스로 GitHub Pages를 사용하면 무료로 호스팅을 할 수 있고 게다가 GitHub Pages 는 Jekyll 을 자체 지원하기 때문에 Jekyll을 사용하여 블로그를 시작한다면 GitHub Pages를 사용하는 것이 좋은 방법일 수 있습니다.
GitHub Pages는 Jekyll을 지원하기때문에 사용자는 Markdown 문서를 약속된 방식으로만 저장소에 올리면 자동으로 웹페이지로 변환시켜주므로 사용자는 블로그를 자동적으로 업데이트 할 수 있게 됩니다. 
또한 위처럼 Jekyll을 사용하기 위해 별도의 설치나 설정이 크게 필요없기 때문에 마음에 드는 Jekyll 테마를 선택하여 수준급 블로그를 바로 시작할 수 있습니다. 더불어 GitHub 자체가 Git을 위한 온라인 저장소를 제공하기 때문에 블로그에 대한 버전 관리 및 백업도 손 쉽게 할 수 있는 점도 큰 장점입니다.


## 저장소 만들기


GitHub에서 새 Repository를 만듭니다. 이때 주의할 점은 반드시 저장소의 이름을 GitHub의 username뒤에 `.github.io`가 붙은 이름으로 짓습니다.
이렇게 해야만 `[username].github.io`의 도메인을 가지는 블로그를 시작할 수 있습니다.
> 물론 `[username].github.io/[projectName]` 처럼 프로젝트 별 페이지를 제작할 수도 있습니다,
> 이런경우 Repository를 굳이 [username].github.io로 명명하지 않아도 됩니다. 


## 마음에 드는 Jekyll 테마 찾기

처음 블로그를 시작하는 방법으로 Jekyll 을 사용한 이유가 바로 여기에 있습니다. 이미 수많은 사용자들이 올린 Jekyll 테마들이 있기때문에 이를 복사하거나 포크(?)하여 블로그를 시작할 수도 있습니다. 그러나 필자는 포크보다는 테마를 다운로드하여 처음부터 하는 방법을 선택했습니다.
Github 에는 이미 [수 많은 Jekyll 테마](https://github.com/topics/jekyll-theme) 들이 공개되어 있습니다. 이들 중에서 선택하여 내 블로그 저장소에 복사해도 되고 아니면 [지킬테마들만 모아놓은 곳](http://jekyllthemes.org)에서 선택하여 사용해도 좋습니다. 혹은 따로 찾은 테마를 사용하여도 무관합니다.


## Jekyll 테마 적용

선택한 Jekyll Theme 파일을 내 저장소에 압축해제한 다음 `_config.yml` 파일의 내용중 다음을 수정합니다.


```
url                      : "https://[username].github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "" # the subpath of your site, e.g. "/blog"
```


저장소의 `_config.yml`을 수정한 후 변경된 내용을 원격 저장소에 push한다면 잠시후 `[username].github.io` url을 통해 선택한 Jekyll테마가 적용된 블로그를 확인 할 수 있습니다.



## `_posts`에 문서 작성하기


Jekyll은 각 블로그 글(post)마다 파일을 따로 구성합니다. 즉, Jekyll 은 각각의 작성된 파일을 각 포스트(게시글)로 변환시켜 보여줍니다. 그러므로 블로그에 글을 등록하기 위해서는 저장소의 `_post`폴더에 포스팅하고자 할 내용을 파일로 저장해야 합니다. Jekyll 은 `_post` 폴더에 있는 파일만 블로그 포스트로 인식합니다.

블로그 포스트 파일명은 `YYYY-MM-DD-post-name.확장자` 형식으로 지정되어야 하며, Markdown으로 작성된 경우 `YYYY-MM-DD-포스트이름.md`로 저장하면 됩니다.
파일 내용 맨 처음에는 글의 제목과 날짜 등을 접어줍니다. 이 형식을 `Front matter`라고 불리며, Jekyll 에서 글의 메타 데이터를 확인하는 방법입니다. 테마에 따라 여기에 들어가는 정보가 조금씩 다를 수 있으니 사용하신 테마 별로 예시를 확인해주세요.


	---
	layout: post
	title: "This is a title"
	subtitle: "This is a subtitle"
	---
	글제목


## 참고

* [지킬 공식 사이트 한글 번역](http://svperstarz.github.io/jekyll-docs-ko/)
* [지킬로 깃허브에 무료로 블로그 만들기](https://nolboo.github.io/blog/2013/10/15/free-blog-with-github-jekyll/)
* [쉽고 빠르게 수준급의 GitHub 블로그 만들기 - Jekyll Remote Theme으로](https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html)