# HackTheBox-Emdee Five For Life

<img src="https://imgur.com/38Q4TnW.png"/>

Нам показывают эту html-страницу со строкой и просят зашифровать ее с помощью MD5, но когда мы пытаемся отправить ее после шифрования, мы получаем сообщение "Слишком медленно".

<img src="https://imgur.com/Cqov6VD.png"/>

Перехватив запрос с помощью `Burp Suite`, мы обнаружили, что нам нужно сделать POST-запрос для отправки хэша MD5

<img src="https://i.imgur.com/iOW0886.png"/>

Кроме того, строка генерируется случайным образом, поэтому нам нужно написать для нее сценарий, и лучший язык для этого - python, поэтому я использовал библиотеки `python requests`, `hashlib` и `BeautifulSoup`

```python
import hashlib
import requests
from bs4 import BeautifulSoup

# Persists cookies across all requests
request = requests.session()
# URL For the challenge page
url = "http://159.65.25.97:31667"
page = request.get(url)

soup = BeautifulSoup(page.content,"html.parser")
# Saving the string to be encrpyted
string_to_encrpyt = soup.select('h3')[0].text

# Caluclating hash of the string
MD5 = hashlib.md5(string_to_encrpyt.encode('utf-8')).hexdigest()

# POST data to send
data = {'hash' :MD5}

# Sending data as POST request
response = request.post(url,data)
print(response.text)
```

<img src="https://i.imgur.com/UOndl6L.png"/>

У нас есть флаг!!!
