pyresponse
==========
**PureResponseClient**  
Python API wrapper for PureResponse PAINT

**MIT License**  
Copyright (c) 2013 Mikael Kohlmyr, Triggered Messaging Ltd  
mikael.kohlmyr@triggeredmessaging.com

[Triggered Messaging](http://www.triggeredmessaging.com),   
[Pure360](http://www.pure360.com)

API Examples  
==========
**Import**  
```python
from pyresponse import PureResponseClient as PureResponseClient
```
==========
**Initialise**  
```python
pureresponse = PureResponseClient()
```
==========
**Authenticate**  
```python
pureresponse.authenticate('username', 'password')
```
==========
**Create list**  
```python
# with single record
pureresponse.create_list('new_list_name', {'email': 'blackhole@example.com', 'name': 'John Doe'})
# with multiple records
pureresponse.create_list('new_list_name', [{'email': 'mars@example.com', 'name': 'John Doe'}, {'email': 'venus@example.com', 'name': 'Jane Doe'}])
```
==========
**Append list**  
```python
# single record
pureresponse.add_person('new_list_name', {'email': 'blackhole@example.com', 'name': 'John Doe'})
# multiple records
pureresponse.add_person('new_list_name', [{'email': 'mars@example.com', 'name': 'John Doe'}, {'email': 'venus@example.com', 'name': 'Jane Doe'}])
```
==========
**Create message**  
```python
pureresponse.create_message('new_message_name', 'subject line', '<h1>Headline</h1><p>body of text</p>')
```
==========
**Send single message**  
```python
pureresponse.send_to_person('new_message_name', 'recipient@example.com')
```
==========
**Send campaign message**  
```python
pureresponse.send_to_list('new_message_name', 'new_list_name')
```
==========
**Get person**  
```python
# by email address
pureresponse.person_by_email('blackhole@example.com')
# by person id
pureresponse.person_by_id('555555555')
```
==========
**Get list**  
```python
# by list name
pureresponse.list_by_name('new_list_name')
# by list id
pureresponse.list_by_id('555555')
```
==========
**Get message**  
```python
# by message name
pureresponse.message_by_name('new_message_name')
# by message id
pureresponse.message_by_id('555555')
```
==========
**Using constants**  
```python
# e.g.
>>> pureresponse.Message.ID
'messageId'
>>> pureresponse.Upload.TYPE
'uploadTransactionType'
# for more constants see ./lib/core.py
```
==========
**Using core methods**  
```python
# e.g.
# replicating behaviour of pureresponse.person_by_id('555555555')
pureresponse.api_core.load(pureresponse.Class.CAMPAIGN_PERSON, {pureresponse.Person.ID: '555555555'})
# replicating behaviour of pureresponse.person_by_email('blackhole@example.com')
loaded = pureresponse.api_core.load_search(pureresponse.Class.CAMPAIGN_PERSON, {pureresponse.Person.EMAIL: 'blackhole@example.com'})
person = pureresponse.api_core.filter_loaded(loaded, {pureresponse.Person.EMAIL: 'blackhole@example.com'}, None)
result = person[0] if len(person) else None
# both methods will return the same structure (a person record)
```
API Concepts and Internals  
==========
**PureResponse Application Interface (PAINT)**  
PAINT is an interface to PureResponse which is accessed through the [Simple Object Access Protocol (SOAP)](http://en.wikipedia.org/wiki/SOAP).
PAINT is not however a traditional SOAP implementation. All calls to the API go to the same handler for PureResponse which then dispatches the call appropriately. This is why all requests in pyresponse are made from [Core.make_request](/lib/core.py) to a single endpoint, but with different input.

**Beans**  

| Bean type | Prefix       | Description                                                     |
| --------- | ------------ | --------------------------------------------------------------- |
| Entity    | `bus_entity` | The representations of the core data in PureResponse            |
| Facade    | `bus_facade` | Proxies for accessing and manipulating Entity and Search beans  |
| Search    | `bus_search` | Define how entities are searchable and format results           |

