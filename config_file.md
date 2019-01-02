# 설정(Configuration)

구슬 애플리케이션 관련 환경 설정은 하나의 configuration 파일 및 환경 변수(env variable) 를 통해 세팅합니다.

기본 세팅(default) 은 환경 변수 세팅을 포함하지 않기 때문에 구슬 애플리케이션은 `<marbles directory>/config/` 디렉토리로부터 cp 를 로드할 것입니다.
이것이 제가 제안하는 방법입니다.

만약 직접 default 값을 변경하고 싶다면

**cp 파일 또는 환경 변수를 수정한 후에 애플리케이션을 재시작해야 합니다.**

### 1. The Config File:

- 이 파일은 매우 작으며, 만들고자 하는 구슬 회사(company) 에 관한 설정을 갖고 있습니다.
	- 구슬 회사의 이름을 정하고, 구슬 소유자 리스트를 정하고, 애플리케이션의 포트(port) 등을 설정합니다.
- 이 파일은 `<marbles directory>/config/marbles_tls.json` 경로에서 찾을 수 있습니다.
- 이 파일만 수정하면 됩니다.
	- Tip: 각 구슬 회사 별로 새 파일을 하나씩 생성하십시오.

**Example JSON**

```json
{
    "cred_filename": "connection_profile_tls.json",
    "use_events": true,
    "keep_alive_secs": 120,
    "company": "맨체스터 유나이티드",
    "usernames": [
        "포그바",
        "퍼거슨",
        "박지성"
    ],
    "port": 3001
}
```

config 파일은 위 예시에서 언급된 모든 필드(fields) 를 가지고 있어야 합니다.
config 디렉토리에 우리가 사용할 수 있는 예시(example) 가 있습니다.
- **잠깐만!!** `cred_filename` 필드는 올바른 creds 파일명으로 세팅되어 있어야 합니다. (해당 creds 파일이 존재하는지 확인하시고 올바른 설정값이 세팅되어 있는지 훑어보십시오.)


**Field 상세**

- `cred_filename` - 블록체인 네트워크를 사용하기 위한 `Credential File` 의 이름. 자세한 사항은 `Credential File` 색션을 참고하십시오.
- `use_events` - `true` 인 경우, 트랜잭션(tx) 이 원장에 commit 될 때 이벤트를 통보받기 위해 SDK 내의 EventHub.js 파일을 사용. `false` 인 경우는 이벤트를 받지 않음.
- `keep_alive_secs` - 초당 재등록 주기. 즉, gRPC 연결 상태를 유지(keep) 하는 기간을 의미한다. 권장값은 120 초이다.
- `company` - 원하는 구슬 회사 이름
- `usernames` - 초기 애플리케이션 구동 시 생성되어져야 하는 구슬 소유자들 명단
- `port` - 구슬 애플리케이션 호스팅 포트

### 2. The Connection Profile:

- 이 데이터는 블록체인 네트워크와 관련된 세팅을 다룹니다. IP주소, 포트번호, 인증서 등을 포함합니다.
- 파일의 버전은 `<marbles>/config/connection_profile_tls.json` 에서 확인가능합니다.
- 만약 env 버전을 사용하고 있다면 이 값을 직접 수정해야 합니다.
	- `> export CONNECTION_PROFILE="{JSON GOES IN HERE}"`
- Tip: 분리된 블록체인 네트워크들을 위해 별도의 파일들을 사용하십시오.

**만약 IBM Cloud Blockchain 서비스를 사용한다면 수동으로 이 파일을 수정할 필요가 1도 없습니다.**.
이미 [체인코드 설치 실습](./install_chaincode.md) 에서 이 파일을 다운로드 또는 복사 붙여넣기 했을 겁니다.

**Example JSON**

```json
{
	"name": "Docker Compose Network",
	"x-networkId": "not-important",
	"x-type": "hlfv1",
	"description": "Connection Profile for an Hyperledger Fabric network on a local machine",
	"version": "1.0.0",
	"client": {
		"organization": "Org1MSP",
		"credentialStore": {
			"path": "./crypto/example"
		}
	},
	"channels": {
		"mychannel": {
			"orderers": [
				"fabric-orderer"
			],
			"peers": {
				"fabric-peer-org1": {
					"x-chaincode": {}
				}
			},
			"chaincodes": [
				"marbles:v4"
			],
			"x-blockDelay": 10000
		}
	},
	"organizations": {
		"Org1MSP": {
			"mspid": "Org1MSP",
			"peers": [
				"fabric-peer-org1"
			],
			"certificateAuthorities": [
				"fabric-ca"
			],
			"adminPrivateKey": {
				"path": "./crypto/example/private.pem"
			},
			"signedCert": {
				"path": "./crypto/example/cert.pem",
			}
		}
	},
	"orderers": {
		"fabric-orderer": {
			"url": "grpc://localhost:7050"
		}
	},
	"peers": {
		"fabric-peer-org1": {
			"url": "grpc://localhost:7051",
			"eventUrl": "grpc://localhost:7053"
		}
	},
	"certificateAuthorities": {
		"fabric-ca": {
			"url": "http://localhost:7054",
			"httpOptions": {
				"verify": true
			},
			"registrar": [
				{
					"enrollId": "PeerAdmin",
					"enrollSecret": "-"
				}
			],
			"caName": null
		}
	}
}
```

**Field 상세**

- `name` - 이 필드의 주요한 목적은 사람들이 아무런 수정없이 default 설정파일을 사용하려고 하는 것을 감지하기 위해서 입니다. 향후에 체크를 위해서 `Place Holder Network Name` 말고 다른 값을 적어 두십시오.
- `x-networkId` - IBM 블록체인 플랫폼 서비스에 의해 생성되는 Unique id. (로컬에 hyperledger fabric 네트워크를 구성할 때는 필요 X)
- `x-type` - 하이퍼레저 콤포저(Hyperledger Composer) 툴에서 무슨 connector 를 사용하지 명시하는 필드이며 일반적으로 `hlfv1`. (Composer 을 사용하지 않는다면 필요 X)
- `client`
	- `organization` - 자신의 애플리케이션에서 사용할 org 명. 이 이름은 `organizations' 오브젝트의 엔트리에 match 됨
	- `credentialStore` - (Optional)
		- `path` - SDK가 사용할 키값 저장소(key value store) 경로. 암호 곤련 파일들을 저장함.
- `channels`
	- `orderers` - 채널에 가입한 orderer 들의 집합. 각각의 이름은 `orderers` 오브젝트의 엔트리에 match 됨.
	- `peers` - 채널에 가입한 peer 들
	- `chaincodes` - 채널에 초기화(instantiated) 된 체인코드들. id 및 버전은 콜론(colon) 으로 분리된다.
	- `x-blockDelay` - orderer 에 의해 생성되는 블록 생성 주기(ms). 이 중요한 속성이 설정되는 대상이 채널임을 유추할 수 있다.
- `organizations`
	- `peers` - org 가 소유한 각 peer 의 키 이름. 이 이름은 `peers` 오브젝트의 엔트리에 match 됨
	- `certificateAuthorities` - org 가 소유한 각 ca 의 키 이름. 이 이름은 `certificateAuthorities` 오브젝트의 엔트리에 match 됨.
	- `adminPrivateKey` - 이 오브젝트는 `path` **또는** `pem` 필드를 포함한다는 점을 참고하십시오.
		- `path`- 관리자(admin) 개인키 경로. 설치 및 초기화 시에 사용된다.
		- `pem` - The admin private key. Used during install and instantiated.
	- `signedCert` - Note this object can contain `path` **or** `pem` fields.
		- `path` - The path to an admin signed certificate. Used during install and instantiate.
		- `pem` - The admin signed certificate. Used during install and instantiate.
- `orderers` - An object. Must have at least 1 entry. You can add more, but currently only the first one will be used.
	- `url` - The gRPC url to reach the orderer. It must include the port.
- `peers` - An object. Must have at least 1 entry. You can add more, but currently only the first one will be used.
	- `url` - The gRPC url to reach the peer. It must include the port.
	- `eventUrl` - The gRPC url to reach event endpoint of the peer. It must include the port and it is different than the discovery port!
- `certificateAuthorities` - An object. Must have at least 1 entry. You can add more, but currently only the first one will be used.
	- `url` - The gRPC url to reach the ca. It must include the port.
	- `registrar` - An object of enroll IDs and secrets for this org.  Used for invokes and queries on chaincode.
		- `enrollId` - A registered user's id on the CA. Can be found in the CA's yaml file.
		- `enrollSecret` - A registered user's secret on the CA. Can be found in the CA's yaml file.
	- `caName` - The CA to use to authenticate the enroll ID.

Once you have edited `connection_profile_tls.json` you are ready to install/instantiate Marbles.

1. Continue where you left off in the [tutorial](../README.md#installchaincode).
