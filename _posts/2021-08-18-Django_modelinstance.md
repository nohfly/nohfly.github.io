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
### `get()`

`get`(_**kwargs_)

Returns the object matching the given lookup parameters, which should be in the format described in  [Field lookups](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#id4). You should use lookups that are guaranteed unique, such as the primary key or fields in a unique constraint. For example:
```python
Entry.objects.get(id=1)
Entry.objects.get(blog=blog, entry_number=1)
```

---
### `filter()`

`filter`(_**kwargs_)

Returns a new  `QuerySet`  containing objects that match the given lookup parameters.
```python
`
The lookup parameters (`**kwargs`) should be in the format described in  [Field lookups](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#id4)  below. Multiple parameters are joined via  `AND`  in the underlying SQL statement.

If you need to execute more complex queries (for example, queries with  `OR`  statements), you can use  [`Q  objects`](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#django.db.models.Q "django.db.models.Q").

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

---
### `save()`

`save`(_**kwargs_)

If  `save()`  is passed a list of field names in keyword argument  `update_fields`, only the fields named in that list will be updated. This may be desirable if you want to update just one or a few fields on an object. There will be a slight performance benefit from preventing all of the model fields from being updated in the database. For example:
```python
product.name = 'Name changed again'
product.save(update_fields=['name'])
```
The  `update_fields`  argument can be any iterable containing strings. An empty  `update_fields`  iterable will skip the save. A value of  `None`  will perform an update on all fields.

---
### `create()`

`create`(_**kwargs_)

A convenience method for creating an object and saving it all in one step. Thus:
```python
p = Person.objects.create(first_name="Bruce", last_name="Springsteen")
```
and:
```python
p = Person(first_name="Bruce", last_name="Springsteen")
p.save(force_insert=True)
```
are equivalent.

The  [force_insert](https://docs.djangoproject.com/en/3.1/ref/models/instances/#ref-models-force-insert)  parameter is documented elsewhere, but all it means is that a new object will always be created. Normally you won’t need to worry about this. However, if your model contains a manual primary key value that you set and if that value already exists in the database, a call to  `create()`  will fail with an  [`IntegrityError`](https://docs.djangoproject.com/en/3.1/ref/exceptions/#django.db.IntegrityError "django.db.IntegrityError")  since primary keys must be unique. Be prepared to handle the exception if you are using manual primary keys.

---
### `update()`

`update`(_**kwargs_)

Performs an SQL update query for the specified fields, and returns the number of rows matched (which may not be equal to the number of rows updated if some rows already have the new value).

For example, to turn comments off for all blog entries published in 2010, you could do this:
```python
>>> Entry.objects.filter(pub_date__year=2010).update(comments_on=False)
```
(This assumes your  `Entry`  model has fields  `pub_date`  and  `comments_on`.)

You can update multiple fields — there’s no limit on how many. For example, here we update the  `comments_on`  and  `headline`fields:
```python
>>> Entry.objects.filter(pub_date__year=2010).update(comments_on=False, headline='This is old')
```
The  `update()`  method is applied instantly, and the only restriction on the  [`QuerySet`](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet")  that is updated is that it can only update columns in the model’s main table, not on related models. You can’t do this, for example:
```python
>>> Entry.objects.update(blog__name='foo') # Won't work!
```
Filtering based on related fields is still possible, though:
```python
>>> Entry.objects.filter(blog__id=1).update(comments_on=True)
```
You cannot call  `update()`  on a  [`QuerySet`](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet")  that has had a slice taken or can otherwise no longer be filtered.

The  `update()`  method returns the number of affected rows:
```python
>>> Entry.objects.filter(id=64).update(comments_on=True)
1

>>> Entry.objects.filter(slug='nonexistent-slug').update(comments_on=True)
0

>>> Entry.objects.filter(pub_date__year=2010).update(comments_on=False)
132
```
If you’re just updating a record and don’t need to do anything with the model object, the most efficient approach is to call  `update()`, rather than loading the model object into memory. For example, instead of doing this:
```python
e = Entry.objects.get(id=10)
e.comments_on = False
e.save()
```
…do this:
```python
Entry.objects.filter(id=10).update(comments_on=False)
```
Using  `update()`  also prevents a race condition wherein something might change in your database in the short period of time between loading the object and calling  `save()`.

---
### `delete()`

`delete`()

Performs an SQL delete query on all rows in the  [`QuerySet`](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet")  and returns the number of objects deleted and a dictionary with the number of deletions per object type.

The  `delete()`  is applied instantly. You cannot call  `delete()`  on a  [`QuerySet`](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet")  that has had a slice taken or can otherwise no longer be filtered.

For example, to delete all the entries in a particular blog:
```python
>>> b = Blog.objects.get(pk=1)

# Delete all the entries belonging to this Blog.
>>> Entry.objects.filter(blog=b).delete()
(4, {'weblog.Entry': 2, 'weblog.Entry_authors': 2})
```

---
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzkwNDA4NTQxLC0yODM0NDI3MTgsNzUyNT
k0OTU1LDEzNDc4NDU3NzgsNzEyNDIwMzgyLC0xNTg5MzQwMjgy
XX0=
-->