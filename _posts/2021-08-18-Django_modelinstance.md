---
title: "Django model instance  내용 정리"
categories:
 - Django
tags:
 - Django
 - Django model
published: true
---

## Django Model Instance
### Specifying which fields to save

If  `save()`  is passed a list of field names in keyword argument  `update_fields`, only the fields named in that list will be updated. This may be desirable if you want to update just one or a few fields on an object. There will be a slight performance benefit from preventing all of the model fields from being updated in the database. For example:
```python
product.name = 'Name changed again'
product.save(update_fields=['name'])
```
The  `update_fields`  argument can be any iterable containing strings. An empty  `update_fields`  iterable will skip the save. A value of  `None`  will perform an update on all fields.

---
### `values()`

`values`(_*fields_,  _**expressions_)

Returns a  `QuerySet`  that returns dictionaries, rather than model instances, when used as an iterable.

Each of those dictionaries represents an object, with the keys corresponding to the attribute names of model objects.

This example compares the dictionaries of  `values()`  with the normal model objects:
```python
# This list contains a Blog object.
>>> Blog.objects.filter(name__startswith='Beatles')
<QuerySet [<Blog: Beatles Blog>]>

# This list contains a dictionary.
>>> Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
```
The  `values()`  method takes optional positional arguments,  `*fields`, which specify field names to which the  `SELECT`  should be limited. If you specify the fields, each dictionary will contain only the field keys/values for the fields you specify. If you don’t specify the fields, each dictionary will contain a key and value for every field in the database table.

Example:
```python
>>> Blog.objects.values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
>>> Blog.objects.values('id', 'name')
<QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
```
The  `values()`  method also takes optional keyword arguments,  `**expressions`, which are passed through to  [`annotate()`](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet.annotate "django.db.models.query.QuerySet.annotate"):
<!--stackedit_data:
eyJoaXN0b3J5IjpbODY5MDk3MTc1LC0xNTg5MzQwMjgyXX0=
-->