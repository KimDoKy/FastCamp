# 2017-01-25 (python setting for MacOS)
-
## pyenv, virtualenv, iPython설치 및 설정

### pyenv
>프로젝트 별로 파이썬 버젼을 따로 관리할 수 있도록 하는 라이브러리

### virtualenv
>파이썬 개발환경을 프로젝트 별로 구분해서 관리할 수 있게 하는 라이브러리  
**pyenv**는 **파이썬의 버전을 관리**해주는 것이며, **virtualenv**는 **파이썬 패키지 설치 환경을 따로 관리**

### pyenv-virtualenv
>pyenv제작자가, pyenv를 사용할 경우 쉽게 virtualenv를 사용할 수 있도록 만든 라이브러리

## pyenv 설치
`brew install pyenv`
`brew install pyenv-virtualenv`

### 기본 셸 변경
#### zsh
>bash(유닉스 기본 shell)와 비슷하게 동작하는 셸   
사용성이 좋습니다. [참고](http://theyearlyprophet.com/love-your-terminal.html)

```
brew install zsh zsh-completions
curl -L http://install.ohmyz.sh | sh
chsh -s `which zsh`
```
>> **`chsh: /usr/local/bin/zsh: non-standard shell` 오류 발생할 경우**
>> 
>> ```
>> sudo vim /etc/shells
맨 아래에 `which zsh`했을때의 결과를 추가 후 저장
```

>현재 shell 확인법
`echo $SHELL`

### pyenv 설정
- 설치 후 pyenv관련 설정을 shell설정에 추가
	- `vi ~/.zshrc`
	
> ```
> export PYENV_ROOT=/usr/local/var/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

pyenv 기본 루트폴더는 `~/.pyenv`  
!!pyenv설정을 shell의 설정파일에 기록 후, 터미널을 재시작하거나 `source ~/.zshrc` 또는 `source ~/.zsh_profile`을 실행


### 파이썬 설치 전 필요 패키지 설치
`brew install readline xz`
>셸에서 방향키 관련 이슈 해결을 위한 유틸리티 설치

### pyenv를 사용해서 파이썬 3.4.3버전 설치
`pyenv install 3.4.3`
>오류시 
>`brew install zlib`  
>오류시
`xcode-select --install`

### 글언벌 선언
`pyenv global 3.4.3`

### 가상환경 생성
`pyenv virtualenv <version> <env name>`
> `pyenv virtualenv 3.4.3 fc-python` 입력

#### 사용할 폴더로 이동 후 local에 가상환경 지정
`pyenv local fc-python`

#### 현재 버전 확인
`pyenv versions`

## iPython
>기본 파이썬 셸보다 다양한 기능을 사용할 수 있도록 해주는 셸을 제공해줌

### iPython 설치
`pip install ipython`

#### iPython 실행
커맨드라인에서 `ipython` 실행
#### iPython 종료
`exit`

-
### pip
> python 패키지 관리자

-

##### vi 단축키

`shift + g`: 가장 아래로
`shift + a` : 현재 줄에서 가장 마지막으로

