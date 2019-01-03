# 체인코드(Chaincode) 개발 환경

다음은 체인코드를 개발하기 위해 설치해야 하는 툴들입니다. (a list of dependencies and recommended tools)

이 툴들은 단순히 옵션이 아니라 **반드시 설치해야 하는 것들입니다.**

## 1. Git

- [참조: Git 다운로드](https://git-scm.com/downloads)

Git 은 소프트웨어 개발에 있어 전세계적으로 가장 인기 있는 버전 관리 툴입니다.
우리는 체인코드 및 애플리케이션 개발 시에 Git 을 사용할 수 있습니다.
오늘 실습에서 Git 이 필요한 이유는 Marbles(구슬) 애플리케이션 코드 및 Hyperledger Fabric 코드를 다운로드 받기 위해  `git clone` 명령어를 사용할 것이기 때문입니다.

### Git 설치하기

제공된 리눅스(ubuntu) 서버에서 다음 명령어를 수행하십시오.

```
$ sudo apt-get install git
```

### Git 설치 확인

설치를 완료하면 다음 명령어를 통해 Git 이 정상적으로 설치되었는지 확인할 수 있습니다:

```
$ git --version
git version 2.11.1.windows.1
```

## 2. GoLang
Go 설치 패키지는 Go CLI 툴을 포함하며, 이는 체인코드를 작성함에 있어 필수입니다.
예를 들어 `go build` 명령어는 자신이 작성한 체인코드를 블록체인 peer 에 디플로이하기 전에, 오류가 없는지 미리 체크할 수 있게 해줍니다.
현재 시점에서 체인코드 빌드를 위한 Go 버전은 `1.9.0+` 이상 버전입니다.

다음 명령어를 수행하여 설치를 진행하십시오.
```
$ sudo apt install golang-go
$ export GOPATH=$HOME/go
$ export PATH=$PATH:$GOPATH/bin
```

Go 설치에 대한 자세한 내용 및 문서, 튜토리얼은 아래 링크를 참조하십시오. 개진철.. 

- [GoLang Download Page](https://golang.org/dl)
- [GoLang Installation Instructions](https://golang.org/doc/install)
- [GoLang Documentation and Tutorials](https://golang.org/doc/)

### Go 설치 확인
아래 명령어를 통해 Go 가 정상적으로 설치되었는지 확인할 수 있습니다. 물론 `go version` 의 출력값은 운영체제에 따라 다를 수 있습니다.

```
$ go version
go version go1.11.1 linux/amd64
```

이번에는 GoLang 으로 간단한 'Hello world' 소스를 작성하여 빌드해 보겠습니다.
먼저 실습을 진행할 디렉토리를 생성한 후 hello.go 파일을 생성하십시오.

```
$ mkdir /home/ubuntu/go
$ cd /home/ubuntu/go
$ vi hello.go
```

이제 아래 샘플을 hello.go 파일에 기입한 후 저장합니다. (putty 콘솔창에서 붙여넣기는 마우스 우클릭)

	```
	package main

	import "fmt"

	func main() {
		fmt.Printf("hello, world\n")
	}
	```

vi editor 가 익숙하지 않은 경우 아래 순서대로 진행할 수 있습니다.
1. `vi hello.go` 로 에디터를 연다.
2. `i' 를 눌러 insert 모드로 전환한다.
3. 위 소스코드를 마우스 우클릭을 통해 붙여넣기한다.
4. `Esc` 를 눌러 일반 모드로 전환한다.
5. `:wq!` 를 입력한 후 엔터를 눌러 에디터를 종료한다.

hello.go 파일 작성이 완료되었다면 빌드를 수행합니다. 

```
$ go build
```  

위 명령어는 자신의 소스코드가 존재하는 디렉토리에 (/home/ubuntu/go) hello.exe 라는 이름의 실행 파일을 생성할 것입니다.
잘 동작하는지 실행해 보십시오.  

```
$ hello
hello, world
```


"hello, world" 메시지를 확인하였다면 Go 설치를 정상적으로 완료한 것입니다.


## 3. Node.js

Node.js 는 자바스크립트 기반의 서버측 프로그래밍 언어입니다. 우리는 본 실습에서 블록체인 네트워크와의 통신을 위해 Node.js SDK 를 이용할 것이며, 사용자 애플리케이션 또한 Node.js 로 작성할 것입니다. 이를 위해 Node.js v8 버전을 설치하겠습니다.
현재 하이퍼레저 패브릭이 지원 및 호환하는 Node.js 버전은 v6 과 v8 입니다.
그리고 본 실습에서 작성할 애플리케이션은 Node.js v9 또는 v10 과 호환되지 않는 점을 유의하시기 바랍니다.

다음 명령어를 통해 Node.js 및 npm (Nodejs Package Manager) 를 설치하십시오.


```
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
apt-get install nodejs
```  

- [참고: Node.js Download Page](https://nodejs.org/en/download/)


###  Node.js 설치 확인

다음 명령어가 정상적으로 수행되는지 확인하십시오.

```
$ node -v
v8.11.4

$ npm -v
6.4.0
```

위의 버전 넘버와 자신의 것이 완전히 일치하는지는 중요하지 않습니다.
명령어가 수행되는지, 그리고 Node.js 의 major 버전이 v8 인지가 중요합니다.


## 4. Hyperledger Fabric

체인코드를 작성하기 위해서는 Hyperledger Fabric 의 shim 라이브러리를 체인코드 소스에 import 해야 합니다.

따라서 로컬 환경에서 체인코드를 컴파일 하기 위해서는 fabric code 를 `GOPATH` 내에 저장해 두어야 합니다.
만약 이 과정을 수행하지 않는다면 로컬 환경에서 체인코드를 빌드할 수 없습니다. 즉 체인코드를 수정할 수 없다는 의미입니다.


### Fabric 설치 가이드
현재 기준으로 IBM Blockchain Platform 상의 Hyperledger Fabric 릴리즈 버전은 1.2 입니다.  
여기서 중요한 점은 개발 환경과 자신의 블록체인 환경 간에 Fabric 버전이 동일해야 한다는 것입니다.  
따라서 개발 환경에서도 동일하게 1.2 버전의 Fabric 을 설치해야 합니다. 

1. GOPATH 내에 parent 디렉토리를 생성하십시오. 
```
$ mkdir -p $GOPATH/src/github.com/hyperledger
$ cd $GOPATH/src/github.com/hyperledger
```

2. Fabric 코드를 다운로드합니다.
```
$ git clone -b release-1.2 https://github.com/hyperledger/fabric.git
```

## 5. IDE 추천 툴

- [Visual Studio Code](https://code.visualstudio.com/#alt-downloads)

Visual Studio Code 는 공짜 무료 IDE 이며, JS/CSS/HTML/GoLang 등과 같이 본 실습에서 사용하는 모든 프로그래밍 언어를 지원합니다.

- [Atom](https://atom.io/)

VS Code 와 같이, Atom 은 본 실습에서 체인코드를 개발하기 위한 모든 언어를 지원하는 플러그인을 갖고 있습니다.

## 6. 마치며
이제 모든 필요한 툴들이 설치되었습니다. 다음 [실습1](../실습1.md) 으로 이동하십시오.
