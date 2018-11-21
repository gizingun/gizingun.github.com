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
>
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

## pyenv
### install
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
### subcommand
>
#### local
> 현재 디렉터리에 python 버전 확인 및 지정
#### global
> 전역으로 설정된 python 버전
#### shell
> shell에 python 버전 지정
#### install
> python-build 를 사용하여 특정 버전의 python 설치 
#### uninstall
> 지정한 버전의 python 삭제 
#### version
> 현재 활성화된 python 버전 출력
#### versions
> pyenv로 설치되어 이용 가능한 버전 출력
#### which
> 활성화된 python 명령의 위치 출력 
#### whenece
> 지정한 명령을 포함하는 모든 python 버전 출력 


## pyenv-virtualenv
### install
homebrew 통해 설치
```
$ brew install pyenv-virtualenv
```
### 환경변수 설정
환경변수 등록
zsh 사용시 "~/.bash_profile"을 "~/.zshrc"로 변경
```
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
```
### 사용법
`pyenv` 로 Python을 설치하면 .pyenv/versions 디렉터리에 2.7.x, 3.4.x 등의 디렉터리가 만들어지고 Python 런타임이 관리 됩니다. virtualenv를 사용하면 동일한 Python 버전을 각기 다른 환경으로 구분하여 관리합니다.

Python 가상 환경 구성
version 을 명시하지 않을 경우 현재 시스템 버전으로 가상환경을 설정
```
$ pyenv virtualenv <vertualenv-name>
or
$ pyenv virtualenv <version> <vertualenv-name>
```
가상환경 종료
```
$ pyenv deactivate
```
가상환경 삭제
```
$ pyenv uninstall {환경명}
```

## `autoenv`
pyenv-virtualenv 은 가상환경을 활성화하기 위해서 직접 activate 해제하기 위해서는 deactivate를 해줘야합니다. 이러한 불편함은 `autoenv`를 통해 해결할 수 있습니다.

### install
zsh 사용시 "~/.bash_profile"을 "~/.zshrc"로 변경
```
$ brew install autoenv
$ echo "source $(brew --prefix autoenv)/activate.sh" >> ~/.bashrc
$ source ~/.bashrc
```
### 사용
autoenv는 프로젝트 디렉토리에 `.env` 파일을 만듦으로써 사용할 수 있습니다.
```
pyenv activate {가상환경명}
```
### Tip
가상환경으로 작업하는 프로젝트 폴더 상위에 아래 내용의 `.env`를 추가
가상환경인 경우에는 해당 가상환경을 deactivate
```
if [ -n "$VIRTUAL_ENV" ] ; then
    deactivate
fi
```

## Dependency 관리

Python은 virtualenv로 dependency를 관리해줄 수 있습니다. 각 프로젝트마다 쓰일 가상 환경(virtual environment)를 생성한 다음 그 프로젝트에 필요한 python을 활성화 한 후에 필요한 라이브러리를 설치하여 사용할 수 있습니다.
가상 환경 안에 설치된 package들은 `freeze` 명령어를 통해서 `requirement.txt` 파일로 추출되고, 이를 통해 새로운 개발환경에서도 손 쉽게 동기화할 수 있습니다.

## 참고
* [TAEWAN.KIM 블로그](http://taewan.kim/post/python_virtual_env/)
* [pyenv 에러 관련](https://github.com/pyenv/pyenv/wiki/Common-build-problems#build-failed-error-the-python-zlib-extension-was-not-compiled-missing-the-zlib)
* [autoenv 설정](https://ujuc.github.io/2017/01/21/autoenv_seor-jeong/)