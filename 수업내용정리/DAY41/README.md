# 41일차 

####실수 리스트 

-  ./manage test 실행 시 create 
{'detail': 'Unsupported media type "multipart/form-data; boundary=BoUnDaRyStRiNg; charset=utf-8" in request.'}
format= 'json'부분을 추가한다. 
```
response = self.client.post(url, data, format='json')
```

- 'MyUser' object has no attribute 'snippets'
	- 8000:member/users 실행 시 나온 오류이다. 

- no such column: snippets_snippet.owner_id
	- 마이그레이션을 안해서 일어난 문제 
	- 디비가 없다는 얘기임.
	- mig를 실행해서 마이그레이션(디비생성)을 한 이후 다시 실행한다. 

- {"detail":"요청된 \"text/plain;charset=UTF-8\"가 지원되지 않는 미디어 형태입니다."}
	- POST로 생성 시 나온 오류. 
	- **Content-type = application/json을 추가한다.** 
---
####더 찾아봐야할 내용 및 질문 

- Python super()에 대한 이해
	- python super는 2가지 유용성을 가진다. 첫 번째로, 상속관계에서 자식클래스가 부모 클래스를 참조할때 명시적(not explicitly name Parent Class' name)으로 부모 클래스의 이름을 나타낼 필요가 없이 super()로 참조하는 것이 가능하다. 
	- **진짜 중요한 것은 multiple inheritance관계에서 python super()가 가지는 특이함으로 dynamic execution env를 지원한다는 것이다.**
	
	- 공식 다큐멘테이션에서 super는 다음과 같이 정의된다. 
	- **Return a proxy object that delegates method calls to a parent or sibling class of type. This is useful for accessing inherited methods that have been overridden in a class. The search order is same as that used by getattr() except that the type itself is skipped.**
	- 2단계 위의 할아버지 클래스를 참조하려면 super([class_name].__base__[0])과 같은 방법이 있다. 
	- Nesting super doesn't work because the type of super isn't the parent type.
	- super는 2개의 argument를 받을 수 있는데 하나는 type이고 하나는 object이다. 이 부분도 이해가 명확하지 않다.
	- MRO(Method Resolution Order)는 파이썬 컴파일시 상속관계에 순서를 정의하는 내용이다? [참조_파이썬_슈퍼튜토리얼](http://www.python-course.eu/python3_multiple_inheritance.php)
	- 다음 부분은 이해가 되지 않는 부분이다. 
> super(), in a class method, is equivalent to super(ReferenceToClassMethodIsBeingDefinedIn, self), where ReferenceToClassMethodIsBeingDefinedIn is determined at compile time, attached to the method as a closure named __class__ and super() will look up both from the calling frame at runtime. But you don't actually need to know all this.

- get_user_model() 함수 이해하기 
```
def get_user_model():
    """
    Returns the User model that is active in this project.
    """
    try:
        return django_apps.get_model(settings.AUTH_USER_MODEL)
    except ValueError:
        raise ImproperlyConfigured("AUTH_USER_MODEL must be of the form 'app_label.model_name'")
    except LookupError:
        raise ImproperlyConfigured(
            "AUTH_USER_MODEL refers to model '%s' that has not been installed" % settings.AUTH_USER_MODEL
        )
```

---

####수업로드 

- 장고 REST 튜토리얼 
	- test작성하기 
	- permission 추가하기 
	- HyperLinked구현
	- 페이지네이션


	
---
####수업진행 포인트

- 어떠한 테스트를 만들것인가?
```
in ft/tests_post.py
class SnippetTest(APILiveServerTestCase):
    def test_snippet_list(self):
        pass

    def test_snippet_create(self):
        pass

    def test_snippet_retrieve(self):
        pass

    def test_snippet_update_partial(self):
        pass

    def test_snippet_update(self):
        pass

    def test_snippet_delete(self):
        pass
```

- browsable api를 쓸수 있기 위한 코드 작성
```
    url(r'^api-auth/',
        include('rest_framework.urls',
                namespace='rest_framework')
        )
```

- 클래스 기반 뷰로 ROOTApi 작성하기 
```
class APIRoot(APIView):
    def get(self, request, *args, **kwargs):
        return Response({
            'users': reverse('member:user_list', request=request, format=format),
            'snippets': reverse('snippet:snippet_list', request=request, format=format)
        })

```

- 뷰셋 작성 (기존의 제네릭 뷰를 모두 합쳐서 한 클래스 표현할 수 있다.

```
class SnippetViewSet(viewsets.ModelViewSet):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
    permission_classes = (
        permissions.IsAuthenticatedOrReadOnly,
        IsOwnerOrReadOnly
    )
    
    @detail_route(renderer_classes=[renderers.StaticHTMLRenderer])
    def highlight(self, request, *args, **kwargs):
        snippet = self.get_object()
        return Response(snippet.highlighted)
    
    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)
```

그러나 대신 urls에서 각 요청에 대해 어떻게 처리할지를 추가적으로 적어줘야 한다. 

- 라우터 활용하기 
```

```