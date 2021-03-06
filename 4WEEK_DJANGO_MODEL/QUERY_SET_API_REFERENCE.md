# QuerySet API reference

index

- QuerySet Evaluation 
- Methods that return new QuerySets

	- filter()
	- exclude()
	- annotate()
	- order_by()
	- reverse()
	- distinct() 
	- values()
	- values_list()
	- select_related()
	- defer()
	- only()
	- using()
	- 
	- 
	
- Methods that do not return QuerySets
	-get()

- Field Lookups 

- Query-related tools	

---
질문사항
1. iterable의 의미 
2. It does not work with child models in a multi-table inheritance scenario.
3. annotate없이도 잘 살 수 있지 않나? 블로그, 엔트리의 예를 들자면, blogs = Blog.objects.all(); for blog in blogs: blog.entry_set.count()로 찍어내면 되지 않나? 



---


####1. QuerySet Evaluation 
- Query set can be constructed, filtered, and so on. However, it never hits the DB until you actually __evaluate it.__

- 그러면 언제 evaluation이 일어나는가? 
	- Iteration
	-Slicing: 보통의 slicing에서는 evaluation이 일어나지 않으나 step을 사용할 경우 발생.
	- repr(): 편의를 위한 것으로 
	- len()
	- bool()
	- list()
	
- Pickling

	- Pickling이란 object를 serializing 시키는 것을 말한다. 
	- 텍스트는 파이썬에서 쉽게 파일로 저장할 수 있으나 그 이외 list, dict, object는 그렇지 않다. 그러한 유형의 객체를 저장하기 위해서 쓰는 것이 바로 피클링이다. 
	
	
####2. QuerySet을 return하는 method 

- filter()
	- __look-up parameter와 일치하는 QuerySet을 반환한다.__
	- field lookup을 활용한 형식으로 작성한다. 
	- 
	
- exclude()
	- __look-up parameter와 일치하는 결과를 제외한 QuerySet을 반환한다.__
	- filter의 반대라고 생각할 수 있다. 
	
- annotate() 
	- 주석을 단다는 의미라기 보다는, it means creating another field for temporary use. 
	- It can be powerful with the use of F(). 
	- annotate내에서도 버전1.8이후 다양한 조건문을 활용하는 것이 가능하다. (When, Case 등)

```
>>> from django.db.models import Count
>>> q = Blog.objects.annotate(Count('entry'))
# The name of the first blog
>>> q[0].name
'Blogasaurus'
# The number of entries on the first blog
>>> q[0].entry__count
42
The Blog model doesn’t define an entry__count attribute by itself, but by using a keyword argument to specify the aggregate function, you can control the name of the annotation:

>>> q = Blog.objects.annotate(number_of_entries=Count('entry'))
# The number of entries on the first blog, using the name provided
>>> q[0].number_of_entries
42
```
위의 예에서 볼 수 있는 것처럼, annotate을 이용하면 Blog에는 각 entry number숫자에 관한 필드가 없지만 그것을 새로 지정하고 그 필드의 이름으로 'number_of_entries'처럼 정할 수 있다. 

다음의 예는 F()와 조합하여 사용된 예이다. 
```
# 앞의 쿼리를 재작성
order_qs = OrderLog.objects.annotate(  
    name=F('product__name'), 
    price=F('product__price')
).values(
    'created', 'name', 'price'
)

# 내용을 보면,
{'price': 9900, 'created': datetime.datetime(2016, 4, 1, 0, 0), 'name': 'ABC Activity'}
{'price': 8200, 'created': datetime.datetime(2016, 4, 1, 0, 0), 'name': '동물동요'}
{'price': 38400, 'created': datetime.datetime(2016, 4, 1, 0, 0), 'name': '사운드북 패키지'}
...
# (하략)
```

- values()
	- Display all the fields and their values in the form of dict. 
	- It can have optional arguments which define what fields to be displayed. 
```
# This list contains a Blog object.
>>> Blog.objects.filter(name__startswith='Beatles')
<QuerySet [<Blog: Beatles Blog>]>

# This list contains a dictionary.
>>> Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
```

- values_list() 
	 - values와 동일하나 values가 사전형태인데 반해 values_list는 튜플형태이다.
	 - 만약 2차원 튜플 형태가 싫다면 flat=True로 1차원 튜플 형태로 만들 수 있다. 
	 - 아래의 예를 참고하자.
	 - 만약에 flat=True이지만, values_list로 지정된 field가 2개 이상이라면 에러가 된다. 왜냐하면 1차원 튜플로 한개의 object에 관해 여러관련 값을 보여준다면, object간의 구분이 어렵기 때문이다.  
```
>>> Entry.objects.values_list('id').order_by('id')
[(1,), (2,), (3,), ...]

>>> Entry.objects.values_list('id', flat=True).order_by('id')
[1, 2, 3, ...]
```

- reverse() 

	- queryset의 순서를 거꿀로 하는 것이다. 
	- 관련 모델에 ordering이 있어야 한다. (ordering이 정의되어 있지 않다면 reverse를 할 기준이 존재하지 않으므로.)
	- 다만 파이썬의 일반적인 seq[-5:]와는 다르게 작동한다. 
	- 장고 DB에서는 말 그대로 거꿀로 세었을 때 맨 뒤의 값부터 보여주는 것이 가능하지만, 맨 뒤의 값 5개 중에서 5번째 값부터 보여주는 seq[-5:]는 지원하지 않는다. 이는 DB상 효율적으로 그렇게 작동할 수가 없기 때문이다. 
	
- distinct()
	- 중복 값을 제외한 queryset을 보여준다. 
	- 일반적으로 쓰일 일이 거의 없다. 
	- order_by와 같이 쓰일 경우에는 의도치 않은 결과를 얻을 수 있다. 

- dates()
	- queryset에 시간 관련(dates)에 관해 필터링을 하고 관련 값을 리턴하는 것. 
	- It takes two required arguments. field should be the name of a DateField of your model. kind should be either "year", "month" or "day"
	- 안에 옵션으로 year, month, day를 지정해줘야 한다.
```
>>> Entry.objects.dates('pub_date', 'day')
[datetime.date(2005, 2, 20), datetime.date(2005, 3, 20)]
```

- select_related()
	- __외래키가 설정된 모델에서__, 외래키로 묶인 다른 모델을 select_related()으로 지정한다면, DB의 부담을 줄일 수 있는 방향의 작동이 가능하다. 

	- filter와의 순서는 크게 상관없다. 
	
아래의 경우에서는 DB를 두번 작동시킨다.
```
# Hits the database.
e = Entry.objects.get(id=5)

# Hits the database again to get the related Blog object.
b = e.blog
```
그러나 아래의 경우는 한번만 작동시켜 더욱 효율적이다. 
```
# Hits the database.
e = Entry.objects.select_related('blog').get(id=5)

# Doesn't hit the database, because e.blog has been prepopulated
# in the previous query.
b = e.blog
```

- select_related(cont.)

	-만약에, 외래키로 묶인 테이블이 3개 이상이라면 selecte_related는 더욱 강력해질 수 있다. 
	- 또한 자기가 얼마나 많은 테이블을 건드릴지 알 수 없다면, you can just have none-argument in which case, the DB searches for all related non-nullable Foreign-key related tables. 
	- 허나 위의 경우는 그렇게 효율적이지 못한 경우가 대부분이다. 



```
Entry.objects.filter(pub_date__gt=timezone.now()).select_related('blog')
Entry.objects.select_related('blog').filter(pub_date__gt=timezone.now())
```


- defer()
	- 특정 필드를 제외하고 QuerySet을 만들어 DB의 부담을 줄인다. 
	- It is for advance use. 
	- 왠만한 경우에는, 테이블을 둘로 나누거나, DB를 정규화하는 것을 생각하는 것이 나을 수 있다. 
	
- only()
	- defer()의 반대로 생각하면 된다. 
	
- order_by()
	- 만약 모델의 ordering으로 어떤 특정한 칼럼으로 정렬할 것을 지정해놨다면 order_by()와 동일한 효과를 얻을 수 있다. 
	- 다른 모델과 연관된 필드로 정렬하려고 한다면,(예를 들어 외래키 등을 활용) 그 모델의 primary_key(id)로 기본적으로 ordering을 한다. 
```
Entry.objects.order_by('blog')
```
위의 결과는 blog의 id로 ordering 한것과 동일한다. 

- using()
	- using은 SQL을 실행할 때 어떤 DB를 활용할 것인지에 관한 것이다. 
	- 그러므로 이는 2개 이상의 DB를 활용할 때에만 의미가 있다. 
	- Its argument only takes __'alias name' not the actual DBname.__
```
# queries the database with the 'default' alias.
>>> Entry.objects.all()

# queries the database with the 'backup' alias
>>> Entry.objects.using('backup')
```
- raw()
	- SQL문을 실행한다. 
	- django.db.models.query.RawQuerySet instance를 리턴한다. 
	- 일반적인 queryset과 마찬가지로 __iterable__하며 이를 통해 instance생성이 가능하다. 

---
	
#### 4. Methods that do not return QuerySets

- get()
	- lookup parameter와 일치하는 record한개를 준다. 
	- 만약 2개 이상이라면 MultipleObjectsReturned 예외를 발생시킨다. 
	- 만약 없다면 DoesNotExist 예외를 발생시킨다. 
	
- create()
	- create은 편의를 위한 것으로 object creation와 save를 동시에 하기 위한 것이다. 
	- 다음의 2가지 예는 동일한 것이다. 
```
p = Person.objects.create(first_name="Bruce", last_name="Springsteen")
and:

p = Person(first_name="Bruce", last_name="Springsteen")
p.save(force_insert=True)
```

- get_or_create()
	- 편의를 위한 것으로 해당 인스턴스를 만들거나 없다면 그것을 생성해내는 것이다. 



- bulk_create()

	- 한꺼번에 여러개를 만들고 싶을 때 이용할 수 있다. 
	- 단 한개의 query로 여러 object를 만들 수 있다. 
	- 제약 사항이 굉장히 많다. 
		- save() 불가 
		- MTM관계에서 사용불가 
		- 다중테이블 상속시 자식 모델에서 사용불가 

- count()
	- QuerySet의 숫자를 보여준다. 
	- 절대 예외를 발생시키지 않는다. 
	- 만약 Entry.objects.count()을 실행시킨다면 Entry의 모든 객체 숫자를 보여준다. 
	- count()는 결국 SQL의 SELECT COUNT(*)과 동일한 것이다. 
	- 만약 메모리에서 무언가를 할 예정이 아니라면 len()보다는 count()를 활용하는 것이 훨씬 효율적이다. 
	
	
#### 4. Field Lookups 

> Field lookups are how you specify the meat of an SQL WHERE clause. They’re specified as keyword arguments to the QuerySet methods filter(), exclude() and get().

고기 같은 녀석들 

- exact: 정확히 같은 단어가 포함될때? 
- iexact: exact와 같으나 대소문자 구별 하지 않음 
- contain: 
- startswith: 해당 값으로 시작하는 밸류 써칭. 
- istartswith: case-insensitive 
- gt(e): greater than의 약자 
- lt(e) 
- 
