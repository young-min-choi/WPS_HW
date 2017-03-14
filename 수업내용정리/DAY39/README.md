# 39일차 

####실수 리스트 

-       if not config[key]:
```
    if not config.get(key):
```


-         except User.DoesNotExist:

---
####더 찾아봐야할 내용 및 질문 

- MODE = 'DEBUG' ./manage.py 실행

> Never deploy a site into production with DEBUG turned on.

>Finally, if DEBUG is False, you also need to properly set the ALLOWED_HOSTS setting. Failing to do so will result in all requests being returned as “Bad Request (400)”.

```
DEBUG = os.environ.get('DEBUG') == 'True'
```
> When you run your app locally with python manage.py runserver, os.environ.get('DEBUG') can be None and therefore the Python DEBUG variable is set to False on your app.

장고가 실행되면 파이썬 환경변수 'DEBUG'에 접근하여 이 값을 True와 비교한다. 만약 debug상태, 즉 개발 상태라면 true이므로 true==true가 true가 되어 debug가 true가 된다. 그러나 반대로 생산 상태라면 이 값은 none이므로 none!= true가 되어 debug가 false가 된다. 



- 예외 처리에서 'as e'의 의미? 
```
try:
    1/0
except Exception as e:
    print '%s (%s)' % (e.message, type(e))
```

as e 는 alias로 긴 에러 변수명을 e로 대체하는 것이다. 
이를 통해 에러변수를 e로 접근하는 것이 가능하다. 


- 장고 모델의 class Meta 옵션중 order_with_respect_to 

> Makes this object orderable with respect to the given field, usually a ForeignKey. This can be used to make related objects orderable with respect to a parent object. For example, if an Answer relates to a Question object, and a question has more than one answer, and the order of answers matters, you’d do this:

인스타그램에서 한 글에 2장 이상의 사진을 올릴 수 있다고 가정하자. 한 글에 여러장의 사진이 있는 경우 각 글마다 사진의 순서가 중요할 것이다. 이런 경우 사용하는 옵션이다. 추가적으로 get_RELATED_order() and set_RELATED_order()라는 메쏘드가 사용가능하다. 


---

####수업로드 

- instagram_api
	- 기본설정
	- db 설정

- postman 실행
	- 컬렉션 폴더 생성
	- 폴더 내에 api 생성 (url입력) 
	- csrf예외 처리 
	
---
####수업진행 포인트

- class Post의 유저설정
```
author = models.ForeignKey(settings.AUTH_USER_MODEL)
AUTH_USER_MODEL = 'member.MyUser'
```

- db 설정 
```
pip install psycopg2
sudo -u postgres createdb instagram_api owner = cym 
```

> Psycopg is the most popular PostgreSQL adapter for the Python programming language. At its core it fully implements the Python DB API 2.0 specifications.

- 디버그 설정 

```
In settings.py, 

MODE = os.environ.get('MODE') == 'DEBUG'

MODE='DEBUG' ./manage.py runserver
```
파이썬의 환경변수에 접근하기 위해 os.environ.get을 활용한다. 

getenv 와 environ.get의 차이 
> os.environ raises an exception if the environmental variable does not exist. os.getenv does not raise an exception, but returns None

- zshrc가 아닌 스크립트로 축약하기 
	- vi ~/.scripts/manage 
	- chmod 755 ~/.scripts/manage
	- ~/.zshrc에 PATH를 추가한다.
	- export CustomScripts="/Users/ym/.scripts"
	- 혹은 export CustomScripts="$HOME/.scripts"
	- export PATH =$PATH:$CustomScripts"
	- manage -d runserver 로 실행 

```
#!/bin/bash
while getopts "d" opt
do
    case $opt in
        d) mode='DEBUG' ;;
    esac
done
manageCommand="MODE='$mode' ./manage.py ${@:OPTIND}"
echo $manageCommand
eval $manageCommand
```


- 예외 처리 2개 
```
        try:
            author_id = request.POST['author_id']
            author = User.objects.get(id=author_id)
        except KeyError:
            return HttpResponse('key "author_id" is required field ')
        except MyUser.DoesNotExist:
            return HttpResponse('No user with typed id {}'.format(request.POST['author_id']))
```

- User =get_user_model()

- @csrf_exempt
	- csrf_token예외처리하기
	
	
-

```
        first_photo = post.postphoto_set.first()
        if first_photo:
            first_photo_dict = {
                'pk': first_photo.pk,
                'photo': first_photo.photo.url,
            }
            cur_post_dict['photo_list'] = first_photo_dict
```

- view에서 post_list보내기

```
def post_list(request):
    post_all = Post.objects.select_related('author')
    post_dict_list = []
    for post in post_all:
        cur_post_dict = {
            'pk' : post.pk,
            'photo_list': [],
            'created_date': post.created_date,
            'author': {
                'username': post.author.username,
                'pk': post.author.pk
                       }
        }
        photo_list = post.postphoto_set.all()
        for post_photo in photo_list:
            cur_photo_dict = {
                'pk': post_photo.pk,
                'photo': post_photo.photo.url,
            }
            cur_post_dict['photo_list'].append(cur_photo_dict)

        post_dict_list.append(cur_post_dict)


    # 각 Post의 정보에 photo_list를 추가하는 부분 시작

    context = {
        'post_list': post_dict_list,
    }

    return JsonResponse(data=context)
```

위와 같이 굉장히 복잡하다. 

위를 모델안에서 처리하면 굉장히 편해진다. 

```
    def to_dict(self):
        ret = {
            'pk': self.pk,
            'created_date':self.created_date,
            'author': self.author.pk,
            'photo_list': [photo.to_dict() for photo in self.postphoto_set.all()]
        }
        return ret
```

- 직렬화 / 역직렬화 개념 