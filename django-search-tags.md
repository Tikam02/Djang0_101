## How to implement Django Search Field and Tags)keywords 


### Django Search using Django Rest Framework

1. Install Django Rest Framework 

```
$ pip install djangorestframework # Rest Framework

$ pip install markdown       # Markdown support for the browsable API.

$ pip install django-filter  # Filtering support

```

2. Add apps in settings.py 

```
INSTALLED_APPS = [
    ...
    'rest_framework',
    'django_filters',
]
```

3. add the filter backend to your settings:


```
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': ['django_filters.rest_framework.DjangoFilterBackend']
}
```

4. The SearchFilter class supports simple single query parameter based searching, and is based on the Django admin's search functionality.
The SearchFilter class will only be applied if the view has a search_fields attribute set. The search_fields attribute should be a list of names of text type fields on the model, such as CharField or TextField.

```
from rest_framework import filters

class UserListView(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    filter_backends = [filters.SearchFilter]
    search_fields = ['username', 'email']
 ```
 
 5. This will allow the client to filter the items in the list by making queries such as:

```
http://example.com/api/users?search=username
```


6. You can also perform a related lookup on a ForeignKey or ManyToManyField with the lookup API double-underscore notation:

```
search_fields = ['username', 'email', 'profile__profession']
```

7. For JSONField and HStoreField fields you can filter based on nested values within the data structure using the same double-underscore notation:

```
search_fields = ['data__breed', 'data__owner__other_pets__0__name']
```

8.By default, searches will use case-insensitive partial matches. The search parameter may contain multiple search terms, which should be whitespace and/or comma separated. If multiple search terms are used then objects will be returned in the list only if all the provided terms are matched.

The search behavior may be restricted by prepending various characters to the search_fields.
```
    '^' Starts-with search.
    '=' Exact matches.
    '@' Full-text search. (Currently only supported Django's PostgreSQL backend.)
    '$' Regex search.
    
    Example - search_fields = ['=username', '=email']
```

    
- [Django Rest Framework Search Filter](https://www.django-rest-framework.org/api-guide/filtering/#searchfilter)

*****

### Addings tags-keywords and Searching it

1. Install Taggit and Taggit Searializer

```
$ pip install django-taggit
$ pip install django-taggit-serializer
```

3. Add taggit and taggit_serializer in apps - settings.py

```
    INSTALLED_APS = (
        ...
        'taggit`,
        'taggit_serializer',
    )

```

5. Import and Add Taggit manager in models.py

```
from django.db import models

from taggit.managers import TaggableManager


class Food(models.Model):
    # ... fields here

    tags = TaggableManager()
```


4. Add taggit serializer to your serializer 

```
from taggit_serializer.serializers import (TagListSerializerField,TaggitSerializer)


class YourSerializer(TaggitSerializer, serializers.ModelSerializer):

    tags = TagListSerializerField()

    class Meta:
        model = tags
```


5. Add tag model in search fields

```
from rest_framework.filters import SearchFilter

class ListBooks( generics.ListCreateAPIView ):
    serializer_class = BooksSerializer
    filter_backends = [filters.SearchFilter]
    search_fields = ['^books','tags__name']
```
  
  
  
#### References 
- [ Django Taggit](https://github.com/jazzband/django-taggit)
- [Dango Taggit Serializer](https://github.com/glemmaPaul/django-taggit-serializer)

    







