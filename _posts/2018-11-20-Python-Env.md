---
layout: post
title: Python 가상 환경
description: "Python 가상 환경"
tags: [python, pyenv, virtualenv]
---

## Python의 버전관리

Python은 2.x와 3.x이 공존하며, 다수의 서브 버전이 존재합니다. 또한 Python 커뮤니티는 엄청난 수의 패키지를 만들고 공유하고 있습니다. 이런 패키지들은 각기 다른 버전의 Python 기반으로 만들어졌습니다. 하나의 개발환경에서 여러 Python 프로젝트를 진행할 경우 라이브러리 간 충돌이 일어나거나 매번 각 환경에 필요한 라이브러리 및 패키지를 재설치 하거나 Python 버전을 바꿔줘야하는 불편함이 발생할 수 있습니다.
이런 문제들은 Python 프로젝트별 독립된 환경을 제공해줌으로써 해결할 수 있습니다.
개별의 독립된 환경을 pyenv, pyenv-virtualenv, autoenv, pip 와 같은 툴로 구성하는 방법을 알아보겠습니다.

## pyenv, pyenv-virtualenv, autoenv, pip
### pyenv
> Python 버전을 관리
> 다양한 버전의 Python 설치 및 관리
### pyenv-virtualenv
> Python 환경(패키지 및 라이브러리)을 가상으로 관리
> `pyenv` 의 확장플러그인 
### autoenv
> `pyenv-virtualenv` 자동화
> 특정 프로젝트 폴더로 이동시 .env 파일 실행하여 가상환경 활성화
### pip
> Python 라이브러리 관리

## Setup pyenv
### OS X
pyenv를 맥 OS X에 설치하기 위해서는 xcode command line tools와 zlib 설치 필수
```
$ sudo xcode-select --install
$ brew install homebrew/dupes/zlib
```
`homebrew`로 `pyenv` 설치
```
$ brew install pyenv
```
#### 환경변수 적용 및 업데이트
환경변수 등록
zsh 사용시 "~/.bash_profile"을 "~/.zshrc"로 변경
```
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
```

## pyenv 사용법
```
pyenv {sub-command} [{params}...]
```
{| class="wikitable"
|+
|-
|Sub command
|comments
|-
|local
|현재 디렉터리에 python 버전 확인 및 지정
|-
|global
|전역으로 설정된 python 버전
|-
|shell
|shell에 python 버전 지정
|-
|install
|python-build 를 사용하여 특정 버전의 python 설치 
|-
|uninstall
|지정한 버전의 python 삭제 
|-
|version
|현재 활성화된 python 버전 출력
|-
|versions
|pyenv로 설치되어 이용 가능한 버전 출력
|-
|which
|활성화된 python 명령의 위치 출력 
|-
|whenece
|지정한 명령을 포함하는 모든 python 버전 출력 
|}

## `pyenv` Python Install Error

```
Build failed: "ERROR: The Python zlib extension was not compiled. Missing the zlib?"
```

On Mac OS X 10.9, 10.10, 10.11 and 10.13, you may need to set the CFLAGS environment variable when installing a new version in order to configure to find zlib headers

```
CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" pyenv install -v 2.7.7
```

If you installed zlib with `Homebrew`, you can set the CPPFLAGS environment variables:
```
CPPFLAGS="-I/usr/local/opt/zlib/include" pyenv install -v 3.7.0
```

Alternatively, try reinstalling XCode command line tools for your OS and when running Mojave or higher (10.14+) you will also [need to install the additional SDK header](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_release_notes#3035624)
```
xcode-select --install
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /

```

## Dependency 관리

Python은 virtualenv로 dependency를 관리해줄 수 있습니다. 각 프로젝트마다 쓰일 가상 환경(virtual environment)를 생성한 다음 그 프로젝트에 필요한 python을 활성화 한 후에 필요한 라이브러리를 설치하여 사용할 수 있습니다.
가상 환경 안에 설치된 package들은 `freeze` 명령어를 통해서 `requirement.txt` 파일로 추출되고, 이를 통해 새로운 개발환경에서도 손 쉽게 동기화할 수 있습니다.

## What is pyenv?

`pyenv`란 여러 버전의 Python을 쉽게 switching 해서 사용할 수 있도록 해주는 도구입니다. 예를 들면 3.x대 Python을 사용하다가 2.x로 switching하여 작업할 수 있습니다.

## Install `pyenv` , `pyenv-virtualenv`
### 설치
```
$ brew install pyenv
$ brew install pyenv-virtualenv
```



### pyenv로 python 설치 가능 버전 확인
```
$ pyenv install --list
```
### pyenv로 특정 버전의 python 설치
```
$ pyenv install 2.7.10
```
### 설치된 버전 확인
```
$ pyenv versions

* system (set by PYENV_VERSION environment variable)
  2.7.10
```
### 버전 변경
```
$ pyenv shell 2.7.10
```

### virtualenv를 통해 특정 버전의 가상환경 생성
```
$ pyenv virtualenv 2.7.10 test_env
```

## 참고
* [TAEWAN.KIM 블로그](http://taewan.kim/post/python_virtual_env/)
# [pyenv 에러 관련](https://github.com/pyenv/pyenv/wiki/Common-build-problems#build-failed-error-the-python-zlib-extension-was-not-compiled-missing-the-zlib)