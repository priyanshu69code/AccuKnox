# Questions for Django Trainee at Accuknox


I am unable to understand the requirements of Q1, Q2, and Q3 of the First Topic. Should I showcase these in the Django application, or should I explain them in the README and provide simple snippets to demonstrate my understanding of the topic?

- I have mentioned Q1, Q2, and Q3 of the First Topic in the **README** file.
- For Topic 2 requirements, I have attached a `Test2.py` file. A code snippet is also given at the bottom.


## Index

- [Topic 1](#topic-1)
  - [Question 1](#question-1-are-django-signals-executed-synchronously-or-asynchronously)
  - [Question 2](#question-2-do-django-signals-run-in-the-same-thread-as-the-caller)
  - [Question 3](#question-3-do-django-signals-run-in-the-same-database-transaction-as-the-caller)
- [Topic 2](#topic-2)
- [Thank You ❤️](#thank-you-for-considering-me-️)

## Topic 1

### Question 1: Are Django Signals Executed Synchronously or Asynchronously?

Yes, Django signals are run **synchronously** by default. This means that the signal handlers, or receivers, are called in the same thread and process as the sender as soon as the signal is transmitted.

Example Snippet:

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import MyModel

@receiver(post_save, sender=MyModel)
def my_model_post_save(sender, instance, created, **kwargs):
    print("Signal received!")
```

**This code will print "Signal received!" as soon as "MyModel" is saved.**

### Question 2: Do Django Signals Run in the Same Thread as the Caller?

Yes, Django signals run on the same **thread** as the caller. This is because they always happen concurrently.

Here is an example code:

```python
import threading
from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import MyModel

@receiver(post_save, sender=MyModel)
def my_model_post_save(sender, instance, created, **kwargs):
    print(f"Signal received in thread: {threading.get_ident()}")

print(f"Saving model in thread: {threading.get_ident()}")
my_instance = MyModel.objects.create(name="Test")
```

**In output, you will see that both print statements have the same thread ID.**

### Question 3: Do Django Signals Run in the Same Database Transaction as the Caller?

By default, Django signals like `post_save` are executed in the same database transaction as the caller. This implies that when a transaction is rolled back, all database modifications performed by signal handlers are also rolled back.

Example Code:

```python
from django.db import transaction
from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import MyModel

@receiver(post_save, sender=MyModel)
def my_model_post_save(sender, instance, created, **kwargs):
    instance.related_model.save()  # Part of the same transaction

try:
    with transaction.atomic():
        my_instance = MyModel.objects.create(name="Test")
        raise Exception("Force rollback")
except Exception:
    pass
```

In this example, if an exception occurs within the `atomic` block, both `MyModel` and any changes made by `my_model_post_save` will be rolled back.

## Topic 2

This is the code snippet for the requirement:

```python
class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {'length': self.length}
        yield {'width': self.width}

# Example usage
rectangle = Rectangle(10, 5)

# Iterating over the rectangle instance
for dimension in rectangle:
    print(dimension)
```

**Output:**

```
{'length': 10}
{'width': 5}
```
# Thank You For Considering me ❤️
