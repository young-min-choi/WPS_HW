#Model Field reference
~~많다 ㅠㅠ~~
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

- FIELD API REFERENCE
---
####1. FIELD OPTION

- __Following options are all avaialble to all fields!__

- NULL
	- Null allows users to insert empty values.
	- However, it is important to set 'BLANK=TRUE'. It is because __string_based_fields accept ' 'data not as NULL making redundancy in DB.__ 
	- Therefore, you should set BLANK = TRUE. 

- BLANK
	- The difference between NULL and BLANK: NULL is purely data-base related. BLANK is validation-related. 
	- Does validation mean larger than DB?
	
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
	- Basically, it is a file-__upload__ field. (to handle __upload__)
	- It takes two optional arguments (upload_to & storage)
	- FileField.upload_to: It provides the way of setting the file-upload-directory and file-name. 
	- FileField.stoage: It is responsible for storage and retrieval. 


	
	
- ImageField
	- __Very similar to FileField.__
	- It check whether the image is valid. 
	- It takes two optional arguments (upload_to & storage)
