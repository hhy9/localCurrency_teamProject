# localCurrency_teamProject
2020 / (모범시민 팀) 지역화폐 가맹점 프로젝트 

저는 프로젝트로 경기도 지역화폐 가맹점 찾기 서비스를 진행하였습니다.\
지난 5월 지역화폐로 지급받은 재난지원금을 시작으로, 지역화폐 사용에 대한 수요가 증가하였습니다. \
하지만 관련 사이트에 가맹점에 대한 정보가 부족하거나 부정확하여 지역화폐 사용에 불편함이 있었습니다. \
특히 지도 서비스가 미흡했습니다. 저희는 프로젝트를 통해 특히 경기도 의정부 지역의 가맹점 위치정보 서비스를 개선하고, \
회원을 대상으로 연령과 성별에 맞춰 지역화폐 사용처를 추천하는 기능을 추가하였습니다.그리고 관리자 부분을 더해서 통계를보여주었습니다.

--------------------------------------------------------------------


### <관리자 담당> -황혜영 (hhy9)
* TeamProject_server_final 
    * admin패키지 부분 
    * post패키지 PostRepositoryImpl - postList/noticeSearch 메서드
    
* TeamProject_client
  * boxes-> admin 패키지 부분
--------------------------------------------------------------------
#### <통계>
#### chart.js(react-chartjs-2) 라이브러리 사용

##### 1.회원통계 
- 지역별 회원의 수 와 성별/나잇대 별로 볼 수 있습니다.
- 기준은 첫 번째로 기간은 2020/01/01~2020/08/31 까지이며, 두 번째로 회원 수는 10,000명의 회원 수를 기준으로 하였습니다.
- 페이지 처음에 들어 가자마자 통계수치가 보일 수 있게 useEffect로 서버에서 데이터를 받아왔습니다. \
  그리고 select 창에 있는 지역을 선택 시 해당하는 value값이 axios로 보내서 지역에 맞는 값을 가져옵니다.
- 서버에서 Jpa Querydsl을 사용하였습니다.
- 나이통계에 관련된 쿼리문을 작성할 때는 회원의 생일 컬럼을 이용하였습니다. \
  LocalDate Class로 현재 날짜의 년도를 구한 후 회원테이블에서 생일 컬럼을 list로 뽑아 온 후 그 split을 사용하여 년도를 가져왔습니다. \
  그 후 현재날짜와  회원들의 생일 년도를 빼서 나온 값을 switch문을 사용하여 각 나이대별로 count 하였습니다. 



##### 2.가맹점통계
- 각 지역별로 그리고 업종별로 분류를 하였는데, 세부업종들을 대분류로 합쳐서 대분류 기준으로 통계를 냈습니다.
- 데이터는 경기도 데이터 드림 에서 지역화폐 가맹점 공공데이터를 가져왔습니다.
- 가맹점 통계에서는 서버에서 쿼리문 작성 시 대분류 업종들을 enum에 넣은 후 enum을 values()메서드를 사용하여 배열에 집어넣고 \
  그 배열을 for문을 사용하여 각 대분류 업종에 해당하는 row(로우)데이터를 카운트하였습니다.

 
##### 3.매출통계
- 월별로 2020/01/01~2020/08/31의 총 매출금액을 나타냅니다. 
- 쿼리문은 매출금액 컬럼을 sum으로 더 한후 데이터 타입인 판매날짜의 컬럼을 groupBy로 month()메서드를 이용하여 월별로 list를 뽑아서 return해주었습니다. 
- axios로 키와 데이터 값을 받아올 때 key와 value 값을 각각 useState로 따로 만들어서 
  서버에서 가져온 값을 forEach로 key와 value값을 따로 넣어준 후 chart.js 라이브러리 사용시에 \
  labels에 keyState값을 data엔 valueState값을 넣어주어서 select로 선택해서 받아오는 데이터가 변경될 때마다 key와 value가 변경되어서 표현되게 해주었습니다.

----------------------------------------------------------

#### <공지사항 & 1:1문의>

##### 1.공지사항
- 공지사항에는 기본적으로 게시판 Spring Rest api를 사용했습니다.
- 글을 작성(CREAT) 할 때는 ReactQuill 라이브러리를 사용하여서 글 작성시 필요한 toolbar들을 추가하였습니다.  
- 글 상세정보(READ)는 react-router-match를 사용하여 클릭 시 url로 클릭한 글에 해당하는 id 값을 넘겨줍니다. \
  그리고 상세페이지에서는 넘어온 id를 match.params를 사용하여 id값을 바로 axios로 넘겨주어서 해당id 에 맞는 내용을 가져오도록 한 후 보여주었습니다.
- 글 수정(UPDATE)은 수정하여 보내기 버튼 클릭 시 DB에 해당 게시글이 수정되어 반영됩니다
- 글 삭제(DELETE) 시 히스토리를 남기기 위해 (delete) deleteYn (삭제여부)로 따로 boolean타입으로 컬럼을 만들어 주어서 \
  삭제여부를 true값으로 변경 후 공지사항 리스트에서는 삭제 여부 false인 것만 불러오게 하고 DB에는 공지사항 글이 남아있는 방법으로 삭제를 구현하였습니다.
  
##### 2. 1:1 문의
- client에서 남긴 문의 글을 관리자가 확인 후 답변 댓글을 다는 형식으로 하였습니다.
- DB에 이미 있는 문의 글에 comment 컬럼을 따로 만들어서 update형식으로 하여 비어 있는 comment컬럼에 관리자의 답변을 추가하는 걸로 구현하였습니다.
- 답변을 하고 나면 답변 완료로 변경됩니다.

----------------------------------------------------------
#### <회원목록 & 신고/가맹점목록>

##### 1.회원목록
- 관리자들이 회원목록을 볼 수 있도록 가입된 모든 회원의 목록을 불러옵니다.
- 회원이름을 클릭 시 해당 회원정보로 넘어갑니다. 회원의 상세정보와 지역화폐 구매목록을 같이 볼 수 있습니다. 

##### 2.신고/가맹점목록
- client가 가맹점 상세정보에서 신고하기를 누를 시 DB에 신고 컬럼에 카운트가 올라가는데 3회이상이면 관리자 메뉴의 가맹점 목록에 표시됩니다. 
- 신고 표시해제 버튼 클릭 시 신고가 해제가 되는데 DB에서 삭제시키는 것 이 아니라 카운트를 0으로 업데이트해서 목록에서 사라지는 방식으로 구현하였습니다.