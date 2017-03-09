170222 숙제
1. 북마크 기능을 만든다.
    검색 결과의 각 아이템에 '북마크하기'버튼을 만들어서 누르면 DB에 저장
2. 북마크 목록 보기 페이지를 만든다.
    북마크한 영상 목록을 볼 수 있는 페이지 구현
**extra
3. 사용자 별로 북마크를 구분할 수 있도록 한다.
4. 검색 결과에서 이미 북마크를 누른 영상은
    '북마크 되어있음' 또는 '북마크 해제'버튼이 나타나도록 한다.
5. 북마크 목록에서 해당 아이템을 클릭 할 경우 유튜브 페이지로 이동하지 않고,
    자체 video_detail페이지를 구현해서 보여주도록 한다.
6. 그외 넣고싶은 기능 마음껏 넣어보기
7. 또는 CSS로 반응형 모바일 만들어보기
8. 로그인/회원가입 만들기
---
####질문사항 

1. ~~return HttpResponseRedirect(request.META.get('HTTP_REFERER'))~~ __DAY32일을 보면 질문사항에 해결함__

2.   <a href="{{ request.META.HTTP_REFERER }}">리스트로 돌아가기</a>
__DAY32일을 보면 질문사항에 해결함__


---
####실수 리스트 

- margin: o auto; 의 오류 
	- 반드시 그 요소가 width를 가지고 있어야 한다. 명시적으로 고정적으로 

- string indices must be integers
	- request.post로 전달된 값은 string이다 !! 


---
- 북마크 모델을 만들어야 할것 같다. 
	- 모델명을 어떻게 할것인가? 
	- 북마크라는 뜻은 다음과 같다. A bookmark is a thin marker, commonly made of card, leather, or fabric, used to keep the reader's place in a book and to enable the reader to return to it with ease.
	- 비디오클립은 어떠한가? Video clips are short clips of video, usually part of a longer recording. The term is also more loosely used to mean any short video less than the length of a traditional television program.
	- MyVideo가 좋을것 같다. 
	- 어떤 필드들이 들어가야 하는가? 일반적인 비디오와 동일하다. 왜냐하면 북마크한 영상을 볼 수 있는 페이지를 구현해야 되기 때문이다. __그대신 query는 필요없다. 왜냐하면 북마크된 영상은 이미 검색어와는 관련성이 떨어지는 영상들이기 때문이다.__ 가 아닌거 같다.. 어떤 검색어로 검색했을때 나온 결과물인지 아는것도 의미가 있을 것 같다. 
	- 마이그레이션을 하자!!! 
	
- 검색된 결과물 옆에 체크박스를 구현하여 북마크를 할 수 있도록 만들자. 
	- 각 결과물 옆에 button을 만든다.
	- 버튼 위치는 썸네일 옆이 된다. 
	- 그전에 잠시 CSS스타일을 전체적으로 손봐주자. 
	- common.scss, variables.scss를 만들어주고 전체적인 컨셉을 다듬었다. //init

- 북마크 뷰를 구현해보자. 
	- 먼저 북마크 버튼을 누르면 POST로 result(해당 비디오 정보)를 보내준다.
	- 같은 뷰 함수에서는 POST입력이 들어오면 이를 감지한다.
	- 함수에서는 result에서 비디오 정보를 추출해 MyVideo객체를 만들고 저장한다. 
	- form을 따로 만들지 않는다. 
	- 대신 input을 히든으로 만들고 이를 제출할 수 있는 버튼을 visible하게 만든다. 
	- 여기까지 해보고 HttpResponse로 입력된 비디오 정보를 출력할 수 있는지 테스트해본다. //init
	
- 뒤질꺼 같다. 
	- 북마크 목록을 볼수 있는 html파일 작성. 
	- base.html작성하여 각 이외의 html파일에서 extend해오기. //init
	- 북마크 보러 가는 버튼 및 링크 생성. //init 
	- 북마크 된 영상을 보여주는 뷰함수 구현
	- admin에 MyVideo등록 및 데이터 2개 입력 
	- 북마크 된 영상 템플릿에서 보여주기 //init 
	- 북마크 템플릿 스타일 정리 //init 

- 사용자 별로 북마크를 구분할 수 있도록 한다.
	- 사용자 이름 띄우기
	- many-to-many로 MyVideo와 MyUser를 연결
	- BookmarkVideo 중간자 모델을 생성
	- 마이그레이션 실시 //init 
	- 생성된 사용자에 대해 테스트 데이터를 입력. 
	- 사용자 별로 나누어보여지게 하기 위해 뷰함수를 수정. 
	- 북마크된 내용이 없으면 "북마크해주세요" 를 보여줌. //init
	- 북마크 클릭 시 중간자 모델 객체를 뷰함수에서 저장할 수 있게 수정함. //init 
	- 북마크 및 돌아가기 클릭시 이전 결과물을 그대로 보여주는 메인페이지로 돌아가기 구현 //init
	
	
- 이미 북마크된 자료이면 다른 버튼 보여주기 
	- 그전에 중복자료 막기 (class Meta: unique_together) 
	