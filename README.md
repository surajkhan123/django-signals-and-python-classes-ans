# django-signals-and-python-classes-ans

Question 1: Are Django Signals executed synchronously or asynchronously?
Answer:
By default, Django signals are executed synchronously. This means that the sender waits for all connected receivers to finish before continuing

#Code
# models.py
from django.db import models
from django.dispatch import receiver
from django.db.models.signals import post_save

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_signal_receiver(sender, instance, created, **kwargs):
    print(f"Signal received for {instance.name}")

# Usage
my_instance = MyModel(name="Test Instance")
my_instance.save()  # Signal will be executed synchronously after save
