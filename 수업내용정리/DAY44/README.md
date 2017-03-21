# 44일차 

####실수 리스트 

-  NoReverseMatch at /post/list/
Reverse for 'list' with arguments '()' and keyword arguments '{}' not found. 0 pattern(s) tried: []

CBV를 만들기 위해서 url을 다음과 같이 설정하고 PostList라는 클래스를 만들었다.

```
urlpatterns = [
    url('^list/$', post_views.post.PostList.as_view(), name='post-list'),
]

class PostList(View):
    def get(self, request):
        post_list = Post.objects.all()
        context = {
            'posts':post_list,
        }
        return render(request, 'post/post_list.html', context)
```
무엇이 문제일까.. 

base.html이 문제였다. 



---
####더 찾아봐야할 내용 및 질문 

- Django REST tutorial에서 serialization관련 읽기

---

####수업로드 

- 인스타그램 
	- CBV로 변환하기

- SWIPER설치 

---
####수업진행 포인트

- .gitignore 파일 변경하기 

```
**/django_app/static/css
```
별 2개는 경로 여러개를 의미한다. 

- 변경하기 
shift + ctrl + R 
```
{% url.*? %}
빈칸으로 변경 
```
- 사진 띄우기 설정 ( {{ MEDIA URL }}을 사용하지 않고 ) 

```
in html

<img src="{{ post_photo.photo.url }}" alt="" class="post-image">

in urls.py

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'post/', include(post_urls)),
    url(r'^api/', include(api_urlpatterns, namespace='api'))
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

- SWIPER설치 

	- bower install swiper 실행 
	- bower init 실행 
	- bower install jquery    
	- bower install jquery --save


그이전에 설치할 것들 
```
$ sudo apt-get install python-software-properties
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ sudo apt-get install nodejs

$ sudo npm install -g bower
```