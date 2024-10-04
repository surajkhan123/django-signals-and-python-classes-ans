# django-signals-and-python-classes-ans

# Question 1: By default are django signals executed synchronously or asynchronously?

=>Answer: Yes,by default, Django signals are executed synchronously. This means that the sender waits for all connected receivers to finish before continuing
Synchronous execution is the default behavior in Django because it ensures that the signal handlers can make changes to the same request/response cycle, or in the case of database operations, they can be sure the transaction is complete before continuing.

# Code Example:
```python
from django.db import models
from django.dispatch import receiver
from django.db.models.signals import post_save

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_signal_receiver(sender, instance, created, **kwargs):
    print(f"Signal received for {instance.name}")
my_instance = MyModel(name="Test Instance")
my_instance.save()  # Signal will be executed synchronously after save

#This demonstrates that Django signals run synchronously by default, as the save() method waits for the signal to complete.
```

# Question 2: Do django signals run in the same thread as the caller?
=>Answer:
Yes, by default, Django signals run in the same thread as the caller. This means that if a Django signal is triggered in one thread, it will execute in that same thread and will not create a new thread for execution unless explicitly made asynchronous.

# Code Example:
```python
import threading
from django.db import models
from django.dispatch import receiver
from django.db.models.signals import post_save

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_signal_receiver(sender, instance, created, **kwargs):
    current_thread = threading.current_thread()
    print(f"Signal received in thread: {current_thread.name} for {instance.name}")
my_instance = MyModel(name="Test Instance")
my_instance.save()  
# Signal will run in the same thread as save
```

# Question 3: By default do django signals run in the same database transaction as the caller?
=> Answer:
Yes, by default, Django signals run in the same database transaction as the caller. This means if the signal is triggered during a database transaction, the signal handler will be part of that same transaction. If the transaction is rolled back, the signal handler's changes will also be rolled back.

Django supports transactional signals, meaning that signal handlers are executed only if the surrounding transaction successfully completes. If the transaction fails and is rolled back, the signal handlers are not called, ensuring data consistency.
# Code Example:

```python
from django.db import models, transaction
from django.dispatch import receiver
from django.db.models.signals import post_save

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def transaction_signal(sender, instance, **kwargs):
    if transaction.get_autocommit():
        print("Running outside a transaction.")
    else:
        print("Running inside a transaction.")

# Usage
with transaction.atomic():  # Starts a new transaction
    my_instance = MyModel(name="Test Instance")
    my_instance.save()  # Signal runs inside the transaction
```
# Topic: Custom Classes in Python

# Description: You are tasked with creating a Rectangle class with the following requirements:

1. An instance of the Rectangle class requires length:int and width:int to be initialized.
2. We can iterate over an instance of the Rectangle class 
3. When an instance of the Rectangle class is iterated over, we first get its length in the format: {'length': <VALUE_OF_LENGTH>} followed by the width {width: <VALUE_OF_WIDTH>}





