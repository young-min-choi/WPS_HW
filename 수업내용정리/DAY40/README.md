# 40일차 

####실수 리스트 
-     
---
####더 찾아봐야할 내용 및 질문 

- pastebin

- ~~URL/URI의 차이점: 동적인 웹이 구성됨에 따라 URI가 활용도가 더 높다~~
	- URI: Uniform Resource Identifier로 가장 상위개념. 문자열을 통해 자원을 식별.
	-URL: Uniform Resource Locator는 리쏘스의 위치를 나타낸다. www.naver.com/doc.pdf가 예시에 해당. 
	- URI는 URL의 개념을 포함하여 자원을 식별하는 문자열로 생각하면 된다. 이는 쿼리가 될 수도 있으며, RESTful구조가 될수도 있다. 

- ~~CRUD란~~
	- Create, Read, Update, Delete의 준말. 
	- persistent storage를 위한 기본적인 4개의 동작을 말한다. 

- ~~Class Based View (generic) 왜 쓰는가~~
	- mixin 을 통해 이미 구현된 기능을 상속받는다 (함수는 상속이 불가능)
	- request.method의 구분을 더 깔끔하게 구현.
	- 내가 만든 클래스를 라이브러리화 해서 범용적으로 쓸수도 있고 프로젝트 내에서 재사용이 가능하다. 

- ~~MixIn이란~~ [stackoverflow](http://stackoverflow.com/questions/533631/what-is-a-mixin-and-why-are-they-useful)
> **A mixin is a special kind of multiple inheritance.** There are two main situations where mixins are used:
1) You want to provide a lot of optional features for a class.
2) You want to use one particular feature in a lot of different classes.

[장고 다큐멘테이션 mixin 종류](https://docs.djangoproject.com/en/1.10/ref/class-based-views/mixins/)

---

####수업로드 

- 장고 REST 튜토리얼 



	
---
####수업진행 포인트

- argument에 사전 넣기 
```
    def create(self, validated_data):
        """
        Create and return a new `Snippet` instance, given the validated data.
        """
        return Snippet.objects.create(**validated_data)
```
검증된 데이터가 title, code, language 등의 키값과 밸류 값의 사전으로 넣으면 
스니펫 인스턴스를 만든다.

- update시 새로받은 값을 넣고 새로운 값이 없으면 기존 값으로 지속, 이를 통해 몇가지 부분만 update하는 것이 가능하다. 
```
def update(self, instance, validated_data):
        instance.title = validated_data.get('title', instance.title)
```

- serializer를 상속하면 반드시 몇가지 method를 구현해야 한다. 
	- update와 create을 반드시 구현해야 한다. 
	
- snippet intance 만들기 
```
serializer = SnippetSerializer(data=data)
serializer.is_valid()
# True
serializer.validated_data
# OrderedDict([('title', ''), ('code', 'print "hello, world"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
serializer.save()
# <Snippet: Snippet object>
```
data를 불러온뒤에
is_valid로 유효한지 검사하고
세이브를 하면 자동으로create을 호출하여
인스턴스가 생성된다. 

- 다음 JSONResponse의 의미 
```
class JSONResponse(HttpResponse):
    def __init__(self, data, **kwargs):
        content = JSONRenderer().render(data)
        kwargs['content_type'] = 'application/json'
        super().__init__(content, **kwargs)
```
HttpResponse의 __init__을 수정한것이다. 


- PUT의 경우 

```
    elif request.method == 'PUT':
        serializer = SnippetSerializer(snippet, data=request.data)
```
PUT 요청인 경우 
SnippetSerializer에 기존 인스턴스와 request.data2개가 들어간다.
기존 인스턴스를 새로운 데이터로 update하는 것이다. 

- PUT or PATCH 
```
# elif request.method == "POST" or request.method =="PATCH" if else False
```

- REST의 핵심중 하나는 REQUEST와 RESPONSE 객체 
	- HttpRequest
	- TemplateResponse

- 뷰클래스 함수에 format지정하기
	- urlpatterns = format_suffix_patterns(urlpatterns)를 지정하면, url뒤에 .json, .html등 다양한 포맷이 올 수 있는 경우에도 다 대처가 가능하다. 
	- def api_root(request, format=None): 

- git commit --amend: 이전 커밋에 덮어쓴다. **푸쉬를 하기전에 해야한다.**

- mixins을 쓴 이후에 메쏘드와 연결하기
```
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
```
get은 list함수를 