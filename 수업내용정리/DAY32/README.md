# 32일차 

#### 실수 리스트 
1. no module named django_app 처음에 migration을 시도하려 할때 나는 오류.


---
####질문 및 더 찾아봐야할 내용 

- ~~JSON파일에 대해 알아보기~~ [외국사이트](https://www.copterlabs.com/json-what-it-is-how-it-works-how-to-use-it/)
	- XML보다 쓰기에 편한 파일 교환형식. 

- selected_related()


- ~~token이란~~ [Wikipedia](https://en.wikipedia.org/wiki/Token)
	- 특정한 키나 문자열. 임시적으로 사용하는 경우가 많다.
	- pageToken의 경우, 어떤 페이지로 접근하기 위한 임시적인 문자열이다. 
	- sessionToken(a session identifier), securityToken, accessToken
	- tokenization (
	- session에 대해서도 추가적으로 공부를 해보자. 

- uri란? 

---

####수업로드 

- 전날 숙제 진행
	- 북마크 기능
	- 북마크 해제기능 
	- 북마크 목록 보기 
- 페이지네이션(리스팅)

- 페이스북 
	- 로그인기능 
	- 사용자 엑세스 / 앱 엑세스 구분하기 
	- 엔드포인트(URL)
---
####수업진행 포인트

- 이전 페이지의 검색결과 값으로 돌아가는 방법 
	- {{request.get_full_path}}

```
<input type="hidden" value="{{request.get_full_path}}">
```

로 같이 보내준다. 
혹은 다음과 같은 방법도 존재한다. 

```
return HttpResponseRedirect(request.META.get('HTTP_REFERER'))
```

- 스트링을 템플릿에서 파이썬 객체로 직렬화 하는 메쏘드를 구현하기
	- 템플릿에서 title, description, url등을 가진 video(for video in videos)라는 것은 객체인가? 템플릿으로 보낼때 다음과 같이 보내기 때문에 결국 사전으로 보내고 이게 템플릿에서 렌더 될때 스트링이 된다?
	- 스트링을 다시 파이썬 객체로 변경하는 것을 씨리얼라이징 이라고 하는데 이를 위해서 쓰이는 방법이 3가지 있다. XML, JSON, YAML이 있고 그중에 JSON이 가장 많이 이용된다. 이 과정을 디코딩, 인코딩이라고 한다. 
	- __스트링을 사전으로 바꿀려면 결국 -> json파일로 만든이후 -> 이것을 다시 사전으로 만들어야 한다.__ 
	
```
    results = []
    results.append(entire_dict)
    results.append(items_dict)
    return results
```

- MyUser대신 AUTH_USER_MODEL쓰기
user = models.ForeignKey(settings.AUTH_USER_MODEL)

- 중간자 모델을 사용하는 경우 반드시 중간자 모델 정보를 입력해야 하므로 

- selected_related() 과 .all()의 차이
	- 성능차이로 이어진다. 
	
- 이미 있는지 확인하는 것은 연산하는 것보다 훨씬 효율적이다. 

- 북마크 기능을위한 뷰의 핵심적인 로직.
```
            token = BookmarkVideo.objects.filter(
                user=user,
                video__videoId=videoId).exists()                    
```

