# django-signals-and-python-classes-ans

# Question 1: By default are django signals executed synchronously or asynchronously?

=>Answer: Yes,by default, Django signals are executed synchronously. This means that the sender waits for all connected receivers to finish before continuing

# Code
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

# Question 2: Do django signals run in the same thread as the caller?
=>Answer:
Yes, Django signals run in the same thread as the caller by default.

# Code:
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


