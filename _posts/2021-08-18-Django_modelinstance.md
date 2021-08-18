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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODkzNDAyODJdfQ==
-->