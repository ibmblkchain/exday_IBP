# IBM 클라우드 블록체인 서비스:

### IBM Cloud 에서 블록체인 네트워크 생성하기 (스타터 플랜)
1. IBM Cloud 에서 블록체인 네트워크를 생성하는 것은 버튼 몇 번과 양식 한 두개만 채우면 쉽게 가능합니다.
IBM 클라우드 서비스는 블록체인 네트워크를 형ㅇ성하는 우리의 피어들과 오더러를 구동시킬 것이다.
또 우리는 다른 네트워크에 가입(join) 할 수 있지만, 지금은 일단 우리 것을 생성하는데 집중하겠습니다.

1. 만약 IBM Cloud 계정을 갖고 있지 않다면 먼저 [IBM ID 를 생성](https://console.ng.bluemix.net/registration/) 하십시오.
2. [IBM Cloud](https://console.ng.bluemix.net) 에 로그인 해주십시오.
3. 화면 상단의 네비게이션 바에서 "카탈로그(Catalog)" 를 클릭하십시오.

![](/doc_images/bluemix_ibc1.png)

1. 검색 창에서 "Blockchain" 으로 검색하여 관련 서비스를 찾으십시오. ~~물론 노가다로 찾으셔도 됨~~

![](/doc_images/bluemix_ibc2.png)

1. 블록체인 타일을 선택하면 서비스 인스턴스 생성 화면이 보일 것입니다. 여기서 블록체인 서비스를 생성할 수 있는데요, 인스턴스명이라던가 서버 지역(Region) / 조직(Org) / 스페이스(Space) 등을 지정할 수 있습니다.
    - 무슨 말인지 잘 모르겠으면 이런 세팅들은 default 로 그냥 놔두셔도 됩니다. IBM Cloud 에서 이 세팅값들은 때때로 중요한 사항이지만, 우리 실습에서는 중요하지 않기 때문입니다.
    
![](/doc_images/bluemix_ibc3.png)

1. "Service Name" 은 자신의 것임을 알 수 있는 이름으로 변경(rename) 해주십시오.
2. 하단으로 스크롤을 내려서 "Selected Plan" 을 **:lollipop: Starter Membership Plan** 으로 변경해 주십시오. ~~잘못하면 청구서 폭탄~~
이 실습은 starter plan 을 기준으로 구성되어 있지만, enterprise plan 이라고 해서 다를 것은 없다는 점 참고해 주십시오.
3. 가장 하단 우측의 "Create" 버튼을 클릭하십시오.

![](/doc_images/bluemix_ibc4.png)

- 만약 생성이 성공적으로 잘 되었다면 위와 같은 화면이 보이셔야 합니다.
- 축하합니다! ~~아직 한 게 없는 건 함정~~ 방금 블록체인 네트워크를 생성하셨습니다! "Launch" 버튼을 눌러서 블록체인 모니터링 UI 로 접속하십시오.
- Hyperledger Fabric 을 로컬에 직접 설치해보신 분은 알겠지만, 블록체인 인프라를 구성하는 작업은 쉽기도 하면서 때로는 매우 어렵습니다. 각 회사 또는 개인은 그에 맞는 환경이 존재하며 요건도 다르기 때문인데요, 특히 처음 설치는 쉬울지 몰라도 환경을 유지하는 것은 더더욱 많은 수작업이 요구됩니다. IBM Cloud 의 블록체인 네트워크는 그러한 수고를 완전히 덜어주기 때문에 매력적입니다. 우리는 몇 번의 클릭만으로 블록체인 네트워크를 생성하였으며 이는 시작일 뿐입니다.
- "Let's get started!" 팝업창(modal) 을 보고 계시죠? 일단은 "Got it" 버튼을 눌러서 스킵하겠습니다.

![](/doc_images/bluemix_ibc5.png)

- "Overview" 페이지를 확인할 수 있습니다. 여기에 자신의 블록체인 네트워크에 존재하는 노드(node) 리스트가 보입니다.

![](/doc_images/bluemix_ibc6.png)

- "Overview" 페이지에서는 자신의 피어(peers), CA 서버(CAs), 오더러(orderers) The overview page is listing out your peers, CAs, and orderers.  You likely have 1 of each.
	- The handy information on this page is the node statuses (hopefully they all say `Running`) and the `View Logs` link which is in the overflow menu at the end of each row (under the `Actions` column).

- We have one more thing to do for the network setup. We need to make a channel.
- If you are using Starter plan than we already created a channel for you called `defaultchannel`. But I'll run you through the process anyway because its a good thing to know and its non-trivial.
    - A channel is used to isolate our blockchain ledger from others on the network.  Later we will have the opportunity to invite members of our network to our channel. For now we just want to make a channel for ourself.
- Click the "Channels" link on the left navigation menu.

![](/doc_images/bluemix_ibc7.png)

- Next click the create "Request Channel" button in the top right. You will see a panel similar to the image below.

![](/doc_images/bluemix_ibc8.png)

- Give your channel a name and description if you want
    - Due to the crazy rules it may take a few minuets to pick a good name.  Stick to lowercase letters, numbers, dashes, and dots.
- Then click "Next"

![](/doc_images/bluemix_ibc9.png)

- We need ourself added as an "Operator". So find your email address and check the Operator box next to it.
	- Operator means we get to vote on changes to this channel.
- If we wanted to invite others to the channel, we could add them from the drop down and select their roles. For now lets only have ourself on this channel.
- Then click the "Next" button

![](/doc_images/bluemix_ibc10.png)

- The update policy decides what it takes to make changes to the channel. In this case how many operators should it take. Since we only have ourself on the channel, the only value that makes sense is 1. Use the dropdown to set this to 1.
- Then click the "Submit Request" button

![](/doc_images/bluemix_ibc11.png)

- OK. So the request is made, but we still have to sign it and submit it.
- In the "Notifications" tab look for a pending request with your channel name on it.
- Open the request by clicking the big giant button in the Actions column called "Review Request"
    - You could review it by expanding each section, but since we made it we know whats inside

![](/doc_images/bluemix_ibc12.png)

- With the notification opened we can now sign the request by clicking "Approve"
	- After clicking "Approve" the notification will close
- Next submit the request with the "Submit Request" button

![](/doc_images/bluemix_ibc13.png)

- Now browse to the "Channels" page in the left navigation menu. It should look similar to the image above.
- If all went well you should see the channel name listed after the panel refreshes

![](/doc_images/bluemix_ibc14.png)

- But we are not quite done... Click the dots in the action column and select the "Join Peers" option
- A menu will appear, check all of your peers and then click the "Join Selected" button
- If the stars align the peer will be joined to your channel and everything is done
	- You can tell it was successful if the date created and block height (on the channels page) have dates and a number, instead of a '-'
	- If you don't see a date, refresh the page or repeat the join

### Finish Up
Congrats! The network is all setup. If you want more detail on the IBM Blockchain service, available plans, or a detailed overview of the IBM Blockchain Dashboard, jump over [here](https://console.ng.bluemix.net/docs/services/blockchain/index.html?pos=2). If not let’s continue the setup.

- Continue where you left off in the [tutorial](../README.md#installchaincode).
