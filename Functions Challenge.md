# Functions Challenge
## Directions

Building off of the dictionaries challenge, create a function that takes a Pleiades url as input and returns a dictionary with two pieces of information: "location_id" and "coordinates." For example, given the url `https://pleiades.stoa.org/places/423025/json` return the dictionary:
`{'location_id': '423025', 'coordinates': [12.486137, 41.891775]}`

Then, use a for loop to apply your function to the list of urls below building a new list of the returned dictionaries called `coordinates_list`. Your final result should look like this:

`[{'location_id': '423025', 'coordinates': [12.486137, 41.891775]}, {'location_id': '550595', 'coordinates': [26.238889, 39.9575]}, {'location_id': '314921', 'coordinates': [10.312944, 36.847009]}, {'location_id': '60406/', 'coordinates': [71.06148, 29.2389543]}, {'location_id': '79574/', 'coordinates': [-0.088949, 51.513335]}]`

Hint: The location id can be found in the url, you will need to isolate it somehow.

Bonus task: Find the location name and include it in your dictionary.



## Solution


```python
import requests
import json
```


```python
url = "https://pleiades.stoa.org/places/423025/json"
```


```python
id = url.split('/')
```


```python
#split the string into different sections on the backslash, id is a list of strings
```


```python
id[4]
```




    '423025'




```python
# How to use requests
url = "www.example.com"  # set your url
response = requests.get(url)  # use requests.get() to get a response object
text = response.text  # extract the text from the response object using the text attributeb
```


    ---------------------------------------------------------------------------

    MissingSchema                             Traceback (most recent call last)

    Cell In[10], line 3
          1 # How to use requests
          2 url = "www.example.com"  # set your url
    ----> 3 response = requests.get(url)  # use requests.get() to get a response object
          4 text = response.text
    

    File ~\miniconda3\Lib\site-packages\requests\api.py:73, in get(url, params, **kwargs)
         62 def get(url, params=None, **kwargs):
         63     r"""Sends a GET request.
         64 
         65     :param url: URL for the new :class:`Request` object.
       (...)
         70     :rtype: requests.Response
         71     """
    ---> 73     return request("get", url, params=params, **kwargs)
    

    File ~\miniconda3\Lib\site-packages\requests\api.py:59, in request(method, url, **kwargs)
         55 # By using the 'with' statement we are sure the session is closed, thus we
         56 # avoid leaving sockets open which can trigger a ResourceWarning in some
         57 # cases, and look like a memory leak in others.
         58 with sessions.Session() as session:
    ---> 59     return session.request(method=method, url=url, **kwargs)
    

    File ~\miniconda3\Lib\site-packages\requests\sessions.py:573, in Session.request(self, method, url, params, data, headers, cookies, files, auth, timeout, allow_redirects, proxies, hooks, stream, verify, cert, json)
        560 # Create the Request.
        561 req = Request(
        562     method=method.upper(),
        563     url=url,
       (...)
        571     hooks=hooks,
        572 )
    --> 573 prep = self.prepare_request(req)
        575 proxies = proxies or {}
        577 settings = self.merge_environment_settings(
        578     prep.url, proxies, stream, verify, cert
        579 )
    

    File ~\miniconda3\Lib\site-packages\requests\sessions.py:484, in Session.prepare_request(self, request)
        481     auth = get_netrc_auth(request.url)
        483 p = PreparedRequest()
    --> 484 p.prepare(
        485     method=request.method.upper(),
        486     url=request.url,
        487     files=request.files,
        488     data=request.data,
        489     json=request.json,
        490     headers=merge_setting(
        491         request.headers, self.headers, dict_class=CaseInsensitiveDict
        492     ),
        493     params=merge_setting(request.params, self.params),
        494     auth=merge_setting(auth, self.auth),
        495     cookies=merged_cookies,
        496     hooks=merge_hooks(request.hooks, self.hooks),
        497 )
        498 return p
    

    File ~\miniconda3\Lib\site-packages\requests\models.py:368, in PreparedRequest.prepare(self, method, url, headers, files, data, params, auth, cookies, hooks, json)
        365 """Prepares the entire request with the given parameters."""
        367 self.prepare_method(method)
    --> 368 self.prepare_url(url, params)
        369 self.prepare_headers(headers)
        370 self.prepare_cookies(cookies)
    

    File ~\miniconda3\Lib\site-packages\requests\models.py:439, in PreparedRequest.prepare_url(self, url, params)
        436     raise InvalidURL(*e.args)
        438 if not scheme:
    --> 439     raise MissingSchema(
        440         f"Invalid URL {url!r}: No scheme supplied. "
        441         f"Perhaps you meant https://{url}?"
        442     )
        444 if not host:
        445     raise InvalidURL(f"Invalid URL {url!r}: No host supplied")
    

    MissingSchema: Invalid URL 'www.example.com': No scheme supplied. Perhaps you meant https://www.example.com?



```python
# The data you get from the pleiades url will techincally be a string even though it looks like a dictionary.
# You can use the json library to convert the string to a dictionary
data = json.loads(text)  # json.loads will convert the string to a dictionary. The "s" on the end of "loads" indicates a string
```


```python
def dictionator(url):
    """get info from pleiades urls"""
    response=requests.get(url)
    text=response.text
    data=json.loads(text)
    #get the id
    location_id= url.split('/')[4]
    #get the coordinates
    coordinates=data['features'][0]['geometry']['coordinates']
    #put in a dictionary
    loc_dict={}
    loc_dict['location_id']= location_id
    #creates key for location id in dicionary
    loc_dict['coordinates']= coordinates
    #return dictionary
    return loc_dict
```


```python
result= dictionator('https://pleiades.stoa.org/places/423025/json')
print(result)
```

    {'location_id': '423025', 'coordinates': [12.486137, 41.891775]}
    


```python
urls = [
    'https://pleiades.stoa.org/places/423025/json',
    'https://pleiades.stoa.org/places/550595/json',
    'https://pleiades.stoa.org/places/314921/json',
    'https://pleiades.stoa.org/places/60406/json',
    'https://pleiades.stoa.org/places/79574/json',
]
```


```python
coordinates_list=[]
for url in urls:
    result = dictionator(url)
    coordinates_list.append(result)
```


```python
coordinates_list
```




    [{'location_id': '423025', 'coordinates': [12.486137, 41.891775]},
     {'location_id': '550595', 'coordinates': [26.238889, 39.9575]},
     {'location_id': '314921', 'coordinates': [10.323056, 36.853056]},
     {'location_id': '60406', 'coordinates': [71.06148, 29.2389543]},
     {'location_id': '79574', 'coordinates': [-0.088949, 51.513335]}]




```python

```
