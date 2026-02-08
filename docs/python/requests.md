# Requests - HTTP Library for Python

Requests is an elegant HTTP library for Python, designed for human beings.

## Installation

```bash
pip install requests
# or
uv add requests
```

## Basic GET Request

```python
import requests

response = requests.get('https://api.github.com/users/octocat')

# Check status
print(response.status_code)  # 200

# Get JSON data
data = response.json()
print(data['login'])  # octocat

# Get raw text
print(response.text)

# Get binary content
print(response.content)
```

## HTTP Methods

```python
import requests

# GET
response = requests.get('https://httpbin.org/get')

# POST
response = requests.post('https://httpbin.org/post', data={'key': 'value'})

# PUT
response = requests.put('https://httpbin.org/put', data={'key': 'value'})

# DELETE
response = requests.delete('https://httpbin.org/delete')

# PATCH
response = requests.patch('https://httpbin.org/patch', data={'key': 'value'})

# HEAD
response = requests.head('https://httpbin.org/get')

# OPTIONS
response = requests.options('https://httpbin.org/get')
```

## Passing Parameters

```python
# URL parameters
params = {'key1': 'value1', 'key2': 'value2'}
response = requests.get('https://httpbin.org/get', params=params)
# URL: https://httpbin.org/get?key1=value1&key2=value2

# You can also pass a list of tuples
params = [('key', 'value1'), ('key', 'value2')]
response = requests.get('https://httpbin.org/get', params=params)
```

## Request Body

### Form Data
```python
data = {'username': 'john', 'password': 'secret'}
response = requests.post('https://httpbin.org/post', data=data)
```

### JSON Data
```python
import requests

json_data = {'name': 'John', 'age': 30}
response = requests.post('https://httpbin.org/post', json=json_data)

# Or manually set content-type
import json
response = requests.post(
    'https://httpbin.org/post',
    data=json.dumps(json_data),
    headers={'Content-Type': 'application/json'}
)
```

### Multipart File Upload
```python
files = {'file': open('report.pdf', 'rb')}
response = requests.post('https://httpbin.org/post', files=files)

# With additional fields
files = {'file': open('report.pdf', 'rb')}
data = {'description': 'My report'}
response = requests.post('https://httpbin.org/post', files=files, data=data)
```

## Headers

```python
headers = {
    'User-Agent': 'My App/1.0',
    'Accept': 'application/json',
    'Authorization': 'Bearer YOUR_TOKEN'
}
response = requests.get('https://api.example.com/data', headers=headers)

# Access response headers
print(response.headers['Content-Type'])
```

## Authentication

### Basic Auth
```python
from requests.auth import HTTPBasicAuth

response = requests.get(
    'https://api.example.com/user',
    auth=HTTPBasicAuth('username', 'password')
)

# Shorthand
response = requests.get(
    'https://api.example.com/user',
    auth=('username', 'password')
)
```

### Bearer Token
```python
headers = {'Authorization': 'Bearer YOUR_ACCESS_TOKEN'}
response = requests.get('https://api.example.com/data', headers=headers)
```

### OAuth
```python
# Use requests-oauthlib for OAuth
from requests_oauthlib import OAuth1

auth = OAuth1('YOUR_APP_KEY', 'YOUR_APP_SECRET',
              'USER_OAUTH_TOKEN', 'USER_OAUTH_TOKEN_SECRET')
response = requests.get('https://api.twitter.com/1.1/account/verify_credentials.json',
                       auth=auth)
```

## Sessions

Sessions allow you to persist parameters across requests (cookies, headers, etc.).

```python
import requests

session = requests.Session()
session.headers.update({'Authorization': 'Bearer TOKEN'})

# All requests using this session will have the Authorization header
response1 = session.get('https://api.example.com/endpoint1')
response2 = session.get('https://api.example.com/endpoint2')

# Cookies are automatically persisted
session.get('https://httpbin.org/cookies/set/sessioncookie/123456789')
response = session.get('https://httpbin.org/cookies')
print(response.json())  # Shows the cookie
```

## Cookies

```python
# Send cookies
cookies = {'session_id': '123456'}
response = requests.get('https://httpbin.org/cookies', cookies=cookies)

# Get cookies from response
response = requests.get('https://httpbin.org/cookies/set/mycookie/123')
print(response.cookies['mycookie'])  # 123
```

## Timeouts

```python
# Timeout after 5 seconds
response = requests.get('https://httpbin.org/delay/3', timeout=5)

# Separate connect and read timeouts
response = requests.get('https://httpbin.org/delay/3', timeout=(3.05, 27))
```

## Error Handling

```python
import requests
from requests.exceptions import HTTPError, Timeout, RequestException

try:
    response = requests.get('https://api.example.com/data', timeout=5)
    response.raise_for_status()  # Raises HTTPError for bad status codes
    
except HTTPError as http_err:
    print(f'HTTP error occurred: {http_err}')
except Timeout as timeout_err:
    print(f'Timeout error occurred: {timeout_err}')
except RequestException as err:
    print(f'Error occurred: {err}')
else:
    print('Success!')
    data = response.json()
```

## Response Object

```python
response = requests.get('https://httpbin.org/get')

# Status code
print(response.status_code)  # 200
print(response.ok)  # True if status code < 400

# Headers
print(response.headers)
print(response.headers['content-type'])

# Encoding
print(response.encoding)  # utf-8

# Content
print(response.text)      # String
print(response.content)   # Bytes
print(response.json())    # Parsed JSON

# URL
print(response.url)

# Request that was made
print(response.request.headers)
```

## Redirects

```python
# By default, requests follows redirects
response = requests.get('http://github.com')
print(response.url)  # https://github.com/ (followed redirect)
print(response.history)  # [<Response [301]>]

# Disable redirects
response = requests.get('http://github.com', allow_redirects=False)
print(response.status_code)  # 301
```

## SSL Verification

```python
# Verify SSL certificate (default)
response = requests.get('https://api.example.com')

# Disable SSL verification (not recommended for production)
response = requests.get('https://api.example.com', verify=False)

# Use custom certificate
response = requests.get('https://api.example.com', verify='/path/to/certfile')
```

## Proxies

```python
proxies = {
    'http': 'http://10.10.1.10:3128',
    'https': 'http://10.10.1.10:1080',
}

response = requests.get('https://httpbin.org/ip', proxies=proxies)
```

## Streaming Downloads

```python
# Download large file
url = 'https://example.com/large-file.zip'
response = requests.get(url, stream=True)

with open('large-file.zip', 'wb') as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
```

## Common Patterns

### REST API Client
```python
import requests

class APIClient:
    def __init__(self, base_url, api_key):
        self.base_url = base_url
        self.session = requests.Session()
        self.session.headers.update({
            'Authorization': f'Bearer {api_key}',
            'Content-Type': 'application/json'
        })
    
    def get(self, endpoint, **kwargs):
        url = f'{self.base_url}/{endpoint}'
        response = self.session.get(url, **kwargs)
        response.raise_for_status()
        return response.json()
    
    def post(self, endpoint, data=None, **kwargs):
        url = f'{self.base_url}/{endpoint}'
        response = self.session.post(url, json=data, **kwargs)
        response.raise_for_status()
        return response.json()

# Usage
client = APIClient('https://api.example.com', 'your-api-key')
users = client.get('users')
new_user = client.post('users', data={'name': 'John'})
```

### Retry with Exponential Backoff
```python
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry

session = requests.Session()
retry = Retry(
    total=5,
    backoff_factor=0.1,
    status_forcelist=[500, 502, 503, 504]
)
adapter = HTTPAdapter(max_retries=retry)
session.mount('http://', adapter)
session.mount('https://', adapter)

response = session.get('https://api.example.com/data')
```

## Tips

!!! tip "Always set timeouts"
    Always set a timeout to avoid hanging requests:
    ```python
    response = requests.get(url, timeout=10)
    ```

!!! tip "Use Sessions for multiple requests"
    Sessions are more efficient when making multiple requests to the same host:
    ```python
    with requests.Session() as session:
        session.get('https://api.example.com/endpoint1')
        session.get('https://api.example.com/endpoint2')
    ```

!!! warning "SSL Verification"
    Never disable SSL verification in production! Only use for development/testing.

## References

- [Official Documentation](https://requests.readthedocs.io/)
- [GitHub Repository](https://github.com/psf/requests)
