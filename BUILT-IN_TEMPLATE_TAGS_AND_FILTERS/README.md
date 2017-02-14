# BUILT-IN TEMPLATE TAGS AND FILTERS 

Index 

- Built-in tag reference
- Built-in filter reference 

---
질문사항
1. spaceless는 왜 쓰죠?
2. 


---
 - autoescape
 	- Controls the autoescape behavior. 
 	- Takes on and off as an argument. 
 	- Exceptions are applied to already-filtered or safe variables 
 	
 - block 
 	- It defines a block which can be overriden in child templates. 
 	
 - comment
 	- 주석달기 
 	- ignore everything between comment and comment-end
 	
 - csrf_token
 	- used with POST method 
 	
 - cycle 
 	- 정해진 리스트 사이를 iterate하며 각 요소를 보여줌.
 	- 다돌면 다시 처음으로 간다. 
	- 안에 변수와 함수를 섞어서 쓰는 것이 가능하다. 

```
 {% for o in some_list %}
    <tr class="{% cycle 'row1' 'row2' %}">...
    </tr>
{% endfor %}
```

- extends 
	- 두가지 방식으로 쓰일 수 있다. 
	- {% extends "base.html" %}
	- {% extends variable %}
	- 두번째 방식은 변수가 스트링이면 그것이 Html 의 이름이 되며 혹은 템플릿 오브젝트면 그것을 이용해 상속한다. 
	- 상대경로가 가능하다. 

```
dir1/
    template.html
    base2.html
    my/
        base3.html
base1.html
In template.html, the following paths would be valid:

{% extends "./base2.html" %}
{% extends "../base1.html" %}
{% extends "./my/base3.html" %}
```

- filter 
	- 블락 사이의 내용을 모두 필터링 한다.
	- |을 통해 여러개의 필터링을 체이닝 할 수 있다. 

```
{% filter force_escape|lower %}
    This text will be HTML-escaped, and will appear in all lowercase.
{% endfilter %}
```

- firstof 
	- False가 아닌 첫번째 값을 보여준다. 
	- In case every variable passed False, you can put a literal string to show. 
	- Everything in the block is auto-escaped. If you don't want that use |safe. 

```
{% autoescape off %}
    {% firstof var1 var2 var3 "<strong>fallback value</strong>" %}
{% endautoescape %}
```

- for 

	- for 반복문을 통해 리스트내의 변수들을 꺼내올 수 있다. 
	- 튜플이나 사전의 경우에도 마찬가지로 적용할 수 있으며 그 예는 다음과 같다. 

```
{% for key, value in data.items %}
    {{ key }}: {{ value }}
{% endfor %}
```

- if

	- if문 내에 필터를 사용하는 것이 가능하다. 
```
{% if messages|length >= 100 %}
   You have lots of messages today!
{% endif %}
```

- regroup 
	- It takes 3 required arguments, name of list, variables to be in list, the attribute to group by. 
	- 


- include 
	- 다른 템플릿을 같이 사용하고 싶을 때 

- now 
	- 현재의 시간을 보여준다.
```
It is the {% now "jS \o\f F" %}
```


- spaceless
	- html tag사이의 빈공간을 모두 없애준다. 

- url 

	- 주어진 뷰에 매칭되는 URL절대 참조값을 보여주며, 추가적으로 변수를 받을 수 있다. 
	- 아래의 예를 참조. 
	- 키워드와 포지셔널을 혼용해서는 안된다. 
```
('^client/([0-9]+)/$', app_views.client, name='app-views-client')
{% url 'app-views-client' client.id %} 
```