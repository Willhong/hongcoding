1대다(One-to-Many) 관계는 데이터베이스 모델링에서 매우 중요한 관계 유형 중 하나입니다. 이 관계는 한 개체가 다른 여러 개체와 연결될 수 있을 때 사용됩니다.

Django의 ORM에서는 **`ForeignKey`** 필드를 사용하여 1대다 관계를 표현합니다.

예를 들어, 한 작가가 여러 책을 쓸 수 있는 상황을 생각해보겠습니다. 이 경우 **`Author`**와 **`Book`** 두 개의 모델이 있을 때, **`Book`** 모델에 **`ForeignKey`** 필드를 사용하여 **`Author`** 모델을 참조하게 할 수 있습니다.

```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)

```

위의 예제에서 **`Book`** 모델은 **`author`** 필드를 통해 **`Author`** 모델을 참조합니다. 이렇게 설정하면 하나의 **`Author`** 객체는 여러 **`Book`** 객체와 연결될 수 있습니다.

**`on_delete=models.CASCADE`**는 **`Author`** 객체가 삭제될 때 관련된 모든 **`Book`** 객체도 함께 삭제되도록 하는 옵션입니다. 다른 옵션으로 **`models.SET_NULL`** (해당 필드를 NULL로 설정), **`models.PROTECT`** (삭제를 방지) 등이 있습니다.

Django의 ORM을 사용하면, 특정 작가와 관련된 모든 책을 쉽게 가져올 수 있습니다:

```python
author = Author.objects.get(name="작가이름")
books_by_author = author.book_set.all()

```

**`book_set`**은 **`Author`** 모델에 자동으로 생성된 역참조 관계의 이름입니다.