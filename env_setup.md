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
```

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

Download and install Node.js v6, or v8.
These are the only supported/compatible versions.
As of 8/28/2018 **Marbles has dependencies that will not work with node.js v9 or v10**.


- [Node.js Download Page](https://nodejs.org/en/download/)


###  Verify Node.js Installation

Open a command prompt/terminal and make sure the following commands work on your machine:

```
$ node -v
v8.11.4

$ npm -v
5.6.0
```

It is not important that your version numbers match the ones shown above.
It is important that the commands printed out compatible verison numbers.

## 4. Hyperledger Fabric

Any chaincode that you write will need to import the chaincode shim from Hyperledger Fabric.
Therefore in order to compile chaincode locally you will need to have the fabric code present in your `GOPATH`.
If you don't do this step you will not be able to build your chaincode locally, which means you will be unable to make modifications.

**Choose 1 option below** (read each before you choose):

- **Option 1:** :lollipop: This option is for those that do not want to modify chaincode.  You will be running marbles as is.
	- There are no steps, you are already done! Head back to the [tutorial](../README.md#downloadmarbles).

- **Option 2:**  This option is for those that want to modify chaincode and use a local Fabric network with the most recent Fabric version
	- [Releases of Hyperledger Fabric](https://github.com/hyperledger/fabric/releases).
	- You will need to find the commit hash of your release. Click the releases link above and find the most recent release.  Click it and the release's commit hash is below the release version, similar to the picture below.
	- Remember this hash and go to the `Continue the Fabric Install Instructions` section below. You will enter the hash there.
	![](/doc_images/release_hash.png)

- **Option 3:** Choose this option if you want to modify chaincode and use the Blockchain Service for my network
	- Get the commit hash from your network or use the hash `ae4e37d`.
		- If you have a network on the IBM Cloud service, then the exact Fabric version can be found in the "Support" tab under the "Release Notes" section of your network's dashboard.
	 - Go to the `Continue the Fabric Install Instructions` section below. You will enter the hash there.


### Continue the Fabric Install Instructions
The release you are cloning locally should match the Hyperledger Fabric network you are using when you deploy your application.
You should apply the choice you made above to the instructions below when doing the `git checkout` step.
The point of all of this is to simply have the same version of Fabric on your local computer as the one running your network.

1. Create the parent directories on your GOPATH
	```
	mkdir -p $GOPATH/src/github.com/hyperledger
	cd $GOPATH/src/github.com/hyperledger
	```

2. Clone the appropriate release codebase into $GOPATH/src/github.com/hyperledger/fabric
	```
	git clone https://github.com/hyperledger/fabric.git
	```

3. Match this version to the **commit hash** of your network/Fabric (the first 7 characters will work)
	```
	cd fabric
	git checkout ae4e37d
	```

4. Confirm the level using git branch. It should show the commit level matching the one you provided.
	```
	git branch
	```
	![](/doc_images/git-branch-out.PNG)

### Verify Fabric Installation
Open a command prompt/terminal and browse to this directory `$GOPATH/src/github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02`

```
$ go build --tags nopkcs11 ./
```

It should return with no errors/warnings.
You should also see that an executable was created in this directory.


Note that the `nopkcs11` tag is important.
PKCS 11 is a Public-Key Cryptography Standard that you are unlikely to have on your system.
**Remember to use this flag as you develop/build your chaincode**.

## 5. IDE Suggestions

- [Visual Studio Code](https://code.visualstudio.com/#alt-downloads)

Visual Studio Code is a free IDE that supports all the languages in Marbles such as JS/CSS/HTML/GoLang.

- [Atom](https://atom.io/)

Like VS Code, Atom has plugins to support any of the languages needed to develop chaincode or modify marbles.

## 6. Finish Up
Now that everything is setup, head back to the [tutorial](../README.md#downloadmarbles).
