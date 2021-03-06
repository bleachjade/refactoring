# Refactoring
## BetterTogether
From my previous year ISP project: [BetterTogether Repo](https://github.com/bleachjade/BetterTogether)
### class ShareFood(), ShareRide(), SharePromotion()
In the `BetterTogether/BetterTogetherApp/models.py` class    
[link to models.py file](https://github.com/bleachjade/BetterTogether/blob/master/BetterTogetherApp/models.py)
consider these code:    
```python
class ShareFood(models.Model):
    """Collecting share food event"""
    location_name = models.TextField('Location Name', default="",max_length=80)
    location = models.TextField('Location', default="",max_length=80)
    description = models.TextField('Description', default="",max_length=100)
    date_time = models.TextField('Date and Time',default=str(formatedDate))
    participants = models.ManyToManyField(Info)
    num_people = models.IntegerField("Number of people", default=2)
    host = models.CharField("Host's Name", default="", max_length=30)
    host_gender = models.CharField('Host Gender (F or M)', max_length=1)
    host_number = models.CharField("Host's Phone Number", max_length=10, default="")
    full = models.BooleanField(default=False)
```
```python
class ShareRide(models.Model):
    """Collecting share ride event"""
    myDate = datetime.now()
    formatedDate = myDate.strftime("%Y-%m-%d %H:%M:%S")
    location_name = models.TextField('Location Name', default="",max_length=80)
    location = models.TextField('Location', default="", max_length=80)
    destination_name = models.TextField('Destination Name', default="",max_length=80)
    destination = models.TextField('Destination', default="",max_length=80)
    description = models.TextField('Description', default="", max_length=100)
    date_time = models.TextField('Date and Time',default=str(formatedDate))
    participants = models.ManyToManyField(Info)
    num_people = models.IntegerField("Number of people", default=2)
    host = models.CharField("Host's Name", default="", max_length=30)
    host_gender = models.CharField('Host Gender (F or M)', max_length=1)
    host_number = models.CharField("Host's Phone Number", max_length=10, default="")
    full = models.BooleanField(default=False)
```
```python
class SharePromotion(models.Model):
    """Collecting share promotion event"""
    location_name = models.TextField('Location Name', default="",max_length=80)
    location = models.TextField('Location', default="",max_length=80)
    brand = models.TextField('Brand', default="",max_length=80)
    description = models.TextField('Description', default="",max_length=100)
    date_time = models.TextField('Date and Time', default=str(formatedDate))
    participants = models.ManyToManyField(Info)
    num_people = models.IntegerField("Number of people", default=2)
    host = models.CharField("Host's Name", default="", max_length=30)
    host_gender = models.CharField('Host Gender (F or M)', max_length=1)
    host_number = models.CharField("Host's Phone Number", max_length=10, default="")
    full = models.BooleanField(default=False)
```
* Refactoring Sign: 
  - having three class with many common fields.
### Refactoring methods to use:
  - Extract Superclass.
  - Pull Up Field.
#### Extract Superclass:
  - Firstly, create a new abstract superclass.
  ```python
  class Share(models.Model):
      class Meta:
          abstract = True
  ```
#### Pull Up Field:
  - try to find a field that use in that three classes then create a field with the same name in the superclass.
  - remove the field from three subclasses.
  ```python
  class Share(models.Model):
      location_name = models.TextField('Location Name', default="",max_length=80)
      location = models.TextField('Location', default="",max_length=80)
      description = models.TextField('Description', default="",max_length=100)
      date_time = models.TextField('Date and Time',default=str(formatedDate))
      participants = models.ManyToManyField(Info)
      num_people = models.IntegerField("Number of people", default=2)
      host = models.CharField("Host's Name", default="", max_length=30)
      host_gender = models.CharField('Host Gender (F or M)', max_length=1)
      host_number = models.CharField("Host's Phone Number", max_length=10, default="")
      full = models.BooleanField(default=False)
      
      class Meta:
          abstract = True
  ```
  ```python
    class ShareFood(Share):
        """Collecting share food event"""
        ... ShareFood methods ...
        
    class ShareRide(Share):
        """Collecting share ride event"""
        myDate = datetime.now()
        formatedDate = myDate.strftime("%Y-%m-%d %H:%M:%S")
        destination_name = models.TextField('Destination Name', default="",max_length=80)
        destination = models.TextField('Destination', default="",max_length=80)
        ... ShareRide methods ...
        
    class SharePromotion(Share):
        """Collecting share promotion event"""
        brand = models.TextField('Brand', default="",max_length=80)
        ... SharePromotion methods ...
  ```
