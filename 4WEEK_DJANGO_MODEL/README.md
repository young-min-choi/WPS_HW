#Model Field reference

INDEX 

- FIELD OPTIONS
	- BLANK
	- NULL
	- CHOICES
	- DEFAULT
	- EDITABLE
	- HELP_TEXT
	- PRIMARY_KEY
	- UNIQUE
	- UNIQUE_FOR_DATE
	
- FIELD TYPES
	- AUTOFIELD
	- BooleanField
	- CharField
	- DateField
	- DateTimeField
	- DecimalField
	- DurationField
	- EmailField
	- FileField
	- ImageField
	- IntegerField
	- TextField
	- URLField
	
- RELATIONSHIP FIELDS
- ForeignKeys
- MTM

- FIELD API REFERENCE

---
질문사항 


1. NULL 과 BLANK
2. UNIQUE가 MTM, OTO에서 불가능한 이유 
3. related_name = '+' 이 굳이 필요한 상황? recursive이외에 
4. FieldFile에서는 파일이 저장된 URL만 저장한다. 그렇다면 실질적으로 파일은 어디에 저장되는 것인가? 
5. API란 무엇인가? 
6. Slug와 URL 필드의 다른점? 




---
####1. FIELD OPTION

- __Following options are all avaialble to all fields!__

- NULL
	- Null allows users to insert empty values.
	- However, it is important to set 'BLANK=TRUE'. It is because __string_based_fields accept ' 'data not as NULL making redundancy in DB.__ 
	- Therefore, you should set BLANK = TRUE. 
	- __NULL=TRUE means accepting None-type object.__

- BLANK
	- The difference between NULL and BLANK: NULL is purely data-base related. BLANK is validation-related. 
	- Does validation mean larger than DB?
	- __' ' is string type object.__
	
- CHOICES

	- Choices consists of tuples. __(value, human readable name)__
	- The best practice is set choices in the model class, and also define suitable named constant for each value. (which is shown below) 
	- Default form widget turns into 'select-box'. 
```
	YEAR_IN_SCHOOL_CHOICES = (
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
)
```


```
MEDIA_CHOICES = (
    ('Audio', (
            ('vinyl', 'Vinyl'),
            ('cd', 'CD'),
        )
    ),
    ('Video', (
            ('vhs', 'VHS Tape'),
            ('dvd', 'DVD'),
        )
    ),
    ('unknown', 'Unknown'),
)
```
The explanation in the documentation for the above 3-dimensional tuples is really shitty. The first element in the tuples mean the group name of the followings __which will be displayed in the 'admin'.__ So, it does not mean much. 



- EDITABLE

	- If set FALSE, not displayed in admin and modelform. 
	- Modelform is something that reduces redundancy when there are closely related model and form.  
	- The default is TRUE. 

- HELP_TEXT 

	- If you put some text in HELP_TEXT, it will be displayed in the form widget. 
	- It is also useful in terms of documentation. 

- UNIQUE

	- If applied, all records in the field should be unique thoroughout the field.
	- __ManyToMany, OneToOne, FileField can't have unique True.__
	- Why? in MTM it is not possible, in OTO it is so self-evident? 

- UNIQUE_FOR_DATE
	- Put this option to be equal to the name of DateTimeField or other things. 
	
####2. FIELD TYPES 
	
- AUTOFIELD

	- It is an INTEGERFIELD that automatically increments. 
	- Not used that often.	
	
- BooleanField

	- TRUE/FALSE field
	- Default form gadget: radio check, which seems so obvious.
	
- CharField

	- It is for small to big size. 
	- Default form gadget: TextInput
	
	
- DateField
	- It represents a date. 
	- It has two important options, auto_now and auto_now_add. 
	- auto_now is useful for last-modified-fields.
	- auto_now_add: The value will be automatically set __when the record is created__
	- It seems so obvious, auto_now_add, auto_now, and default are mutually exclusive. 
	- Default form gadget: TextInput
	
- DateTimeField

	- __Exactly same as DateField except it also includes time.__
	
	
- DecimalField

	- It has two required options, max-digits and decimal-places(소수점)
	- It is possible to input number that is beyond decimal places and max-digits. However, you will find out, __the DB will automatically converts that to the set condition.__
	
- EmailField
	
	- It is all the same with CharField except it used emailvalidator to check whether it is valid e-mail address. 
	
- FileField (*)
Before delving into the details, let's think what will be needed for Django to handle File Input and Storage. First, it will need path to save the file and may need specific filename (if it does not want to use the original filename) 

	- Basically, it is a file-__upload__ field. (to handle __upload__)
	- It takes two optional arguments (upload_to & storage)
	- FileField.upload_to: It provides the way of setting the file-upload-directory and file-name. 
	- FileField.stoage: It is responsible for storage and retrieval. 
	- __Django does not store files in DB.__

- (*) 3 steps for file storing and using  
	1. Setting MEDIA_ROOT: This will be the base directory.
	2. Setting upload_to: The path produced by upload_to will be added to MEDIA_ROOT
	3. Using the url path in template. For example, 
```
class Car(models.Model):
	car_manual = models.FileField(upload_to=url_name)
```
You can use {{ object.car_manual.url }} as an absolute path of url. 
한마디로 MEDIA_ROOT은 파일이 저장될 URL이 시작되는 기본 디렉토리 주소이다. 여기에 추가적으로 upload_to에 불러진 값이나 함수(callable이라고도 불림)에 따라 추가적인 주소가 붙는다. 


	
	
- ImageField
	- __Very similar to FileField.__
	- It check whether the image is valid. 
	- It takes two optional arguments (upload_to & storage)

- SlugField
	- It is basically CharField.
	- It only permits letters, numbers, underscores, and hypens.

- URLField 

	
####3. ForeignKey 

- Overview
	- Used in __One To Many Relationship__
	- Its required argument is only one. __the class to which the model is related__
	- If you want to make a recursive, set models.ForeignKey('self')
	- What if the Model you would like to reference is not defined yet? __Use the 'name' instead of name.__ For example, 
```
	class Car(models.Model):
    manufacturer = models.ForeignKey(
        'Manufacturer',
        on_delete=models.CASCADE,
    )
```

- ForeignKey & Abstraction(or Inheritance)

```
products/models.py
from django.db import models

class AbstractCar(models.Model):
    manufacturer = models.ForeignKey('Manufacturer', on_delete=models.CASCADE)

    class Meta:
        abstract = True
production/models.py
from django.db import models
from products.models import AbstractCar

class Manufacturer(models.Model):
    pass

class Car(AbstractCar):
    pass

# Car.manufacturer will point to `production.Manufacturer` here.
```


- What if the object you would like to reference via ForeignKey is deleted? You can have several actions set in prior. 
	
	- Following is the list of actions possible. 
	- CASCADE: Delete the related objects all together. 
	- PROTECT: Delete is not acceptable. It raises an exception. 
	- SET_NULL: The attribute should be set NULL= TRUE at the same time.
	- SET_DEFAULT: The attribute should be set DEFAULT value at the same time.
	
- limit_choices_to 

	- A limit is put on the choices. 
	- A callabe is possible for the value. 
	- For example, 
```
def limit_pub_date_choices():
    return {'pub_date__lte': datetime.date.utcnow()}

limit_choices_to = limit_pub_date_choices
```



- related_name & related_query_name 

	- related_name is used for the referenced objects to backward reference the class that has ForeignKey. 
	- If you don't want backward relations,(or backward referencing 역참조), just set related_name = '+'
	
	- related_query_name is related to backward reference and filter operator. 
	- 왜 굳이 역 참조 필터에서 다른 이름이 필요한지?(질문)

> related_query_name (the name to use for the reverse filter name from the target model)

```
# Declare the ForeignKey with related_query_name
class Tag(models.Model):
    article = models.ForeignKey(
        Article,
        on_delete=models.CASCADE,
        related_name="tags",
        related_query_name="tag",
    )
    name = models.CharField(max_length=255)

# That's now the name of the reverse filter
Article.objects.filter(tag__name="important")
```


####4. ManyToMany 

- When creating MTM, behind the scene, Django creates intermediary table. (다대다를 만들면 사용자가 따로 중간자 테이블을 만들기 전까지, 장고가 보이지는 않지만 두 테이블을 연결하는 중간자 테이블을 자체적으로 만든다.)

- Recursive & Symmetrical 
	- When MTM is set on self, the default value of symmetrical is TRUE.
	- It does not even add (automatic-created) field_set. If you look at the example below, it will sound so obvious. 


```
from django.db import models

class Person(models.Model):
    friends = models.ManyToManyField("self")
```
위의 예를 본다면 사람 클래스 내에서 자기 자신한테 MTM을 걸 경우, 역 참조시, person1.person_set.all()을 생각해 본다면 매우 어색하다는 것을 알 수 있을 것이다. 
	
####5. Field API Reference 

필드란 무엇인가? 필드는 기본적으로 추상화 클래스이다. 필드는 장고에 의해 DB의 칼럼으로 맵핑된다. 모델에서는 필드가 클래스의 속성(class' attribute)이 된다.

__Like CharField, every built-in field is the implementation of Field class.__