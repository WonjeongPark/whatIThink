## 아이디어


<br>
`react-native` 로 앱을 만들어볼까..갑자기 도전의식이 생기면서 설렌다.. 디버깅지옥이 눈에 보이는건 기분탓...(외면)<br>


### A. 타투관련 웹앱<br>
`타투 관련 정보제공`<br>
1. 타투이스트와 타투받고싶은 사람의 소통채널(chat + drawing)<br>
-> howto처럼 단순프로필 나열이 아니라 유형, 포트폴리오 등으로 검색, 구성하는 방법 구체적으로 생각<br>
2. 타투 시안을 미리 카메라를 통해 내 몸에 올려보고 전체적인 크기나 색을 정할 수 있게(webAR, webVR)-> 기술적으로 가능한지 체크<br>
3. 사람의 몸의 일반적인 굴곡이 있으니 AR로 신체를 파악해서 그 위에 시안을 증강시키지 못하더라도 어느정도 적용 가능하지 않을까?->체크 <br>
4. 악세서리나 네일아트 등으로도 적용 가능? <br>
5. 사람들이 쉽게 이용할 수 있도록 앱형태로 제작? ->manifest, pwa 등으로 극복하는방안 체크 // react native?<br>
[react와 react native](https://velog.io/@honeysuckle/React-Native%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%A5%BC-%EC%A7%84%ED%96%89%ED%95%98%EA%B8%B0%EC%A0%84-%EA%B3%A0%EB%A0%A4%EC%82%AC%ED%95%AD-%EB%8B%A8%EC%A0%90-%EC%95%84%EB%8B%98#1.-react%EB%A1%9C-%EC%9B%B9-%EA%B0%9C%EB%B0%9C%ED%95%B4-%EB%B4%A4%EC%9C%BC%EB%A9%B4-%EB%B0%94%EB%A1%9C-%EC%8B%9C%EC%9E%91-%ED%95%A0-%EC%88%98-%EC%9E%88%EC%8A%B5%EB%8B%88%EA%B9%8C), 
[react native 튜토리얼](https://yuddomack.tistory.com/entry/1React-Native-%EC%84%A4%EC%B9%98%EC%99%80-%EC%8B%A4%ED%96%89hello-world?category=754156)
6. 제대로 redux를 사용해보거나, 새로운 js 라이브러리 활용 등 생각.
7. 어떻게든 답을 찾는 과정보다 더 효율적이고 간단한 로직을 짜는 것에 집중해보기.
8. 타투관련 앱은 비슷하게나마 몇가지존재, 피어싱내용과 함께 body art(?)쪽으로?
<-> 아직은 타투가 합법도 아니고 (정확히는 의료 면허가 있는 타투이스트들이 거의 없기때문) 관련 내용 얻기가 힘들 것 같기도하고..

### B. 사진으로 하루 기록하기

캘린터에 사진 등록하면 해당 일의 칸은 사진으로 꽉참, 클릭하면 내가 기록한 시간이나 감상이나 등등 볼 수 있음.
이미 비슷한 어플이 존재함, 내가 원하는 구체적 기능은 무엇인지?

### C. 기프티콘 관리
선물받은 기프티콘을 유효기간, 유형 등으로 정리해서 알람, 사용추천 등 관리하기 -> 이미지 인식 가능? 사용하려면 app이어야..

### D. 음식꿀조합
(친구에게 제공받은 아이디어 + 비슷한 생각을 한 적이 있기에 아이디어노트에 추가)<br>
1. 각 프랜차이즈 별 신메뉴, 커스텀 꿀조합을 공유할 수 있는 어플.<br>
ex) 서브웨이 소스 꿀조합, 스타벅스 엑스트라 꿀조합, 스벅 시즌메뉴 등<br>
2. 각 프랜차이즈 신메뉴 소개
3. 기프티콘 관리서비스와 합쳐서 프랜차이즈에 관한 정보 받는 어플로?<br>
메뉴, 맵기, 칼로리 등으로 검색 할 수 있도록 + 그 결과가 내가 보유한 기프티콘 사용가능한 브랜드라면 그것도 <br>
4. 내 맛집 관리 -위치(카메라로 GPS AR?) 메뉴 평가 등 저장가능하게?

1. 프렌차이즈/비프렌차이즈 음식 꿀조합 제공 (유저들도 올리도록)
--> 베스트 추천은 보상?
2. 프렌차이즈 신메뉴 소개 및 메뉴 정보 제공
3. 각 지점별 이벤트 소개
4. 위치로 프렌차이즈 인식(여러개라면 당신이 있는 곳은?) -> 보유 기프티콘이나 이벤트, 쿠폰 등 제공

--> 유저 참여 유도, 기업 연계, 유저 소통 있음 

### F. 스페인어
출퇴근시간을 이용해서 언어공부.<br>
가만히 앉아서 핸드폰을 들여다 보는 것이 아니라 걷거나, 사람많은 대중교통 속에 있는 경우에도<br>
그 시간을 활용해서 공부할 수 있게.<br>
하나의 대화를 문장단위로 재생 컨트롤(톡- 문장재생, 톡톡 다음문장)<br>

### G. howto +a
HowTo 의 어플 버전 --> PT중계+가격제안시스템+헬스장 시설, 직원, GX, PT등에 대한 평 공유

### H. 맨스뷰티
화장품--> 맨스 뷰티 정보공유, 제품별 평가, 제품 판매 하는 어플.<br>
1. 남자들을 위한 화장품 정보 제공
2. 남자들끼리 화장품 추천 및 꿀팁 제공
3. 영상편집이나 효과 제공? 웹사이트 동시 제공
4. 나중에 제품 판매로 연계

--> 유저 참여 유도, 기업연계, 유저 소통있음, 초기 정보 많이 필요

