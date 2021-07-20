## BeautifulSoup

1. 관련 라이브러리 설치
    - pip install requests
    - pip install beautifulsoup4
2. import

    ```python
    import requests
    from bs4 import BeautifulSoup
    ```

3. requests를 활용하여 웹 내부 소스 가져오기

    ```python
    page = requests.get(url)
    page.text # 해당 페이지의 소스 볼수있음
    ```

    - 번외로 urllib.request, urllib.parse 활용하여서도 내부 소스 가져올 수 있음.
    - response = urllib.request.urlopen(url), page = response.read()
 
4. BeautifulSoup 생성

    ```python
    soup = BeautifulSoup(page.text, 'html.parser') # requests
    soup = BeautifulSoup(page, 'html.parser') # urllib
    ```
    

## 사용법
  - 필자는 find, find_all 두가지 함수만 사용해 보았음.
  - find의 경우는 찾는 한가지만 리턴
  - find_all의 경우는 찾는 모든 케이스 리턴

### find, find_all
  - find(찾는 태그, 찾는 태그의 속성(사전형))
  - find_all(찾는 태그, 찾는 태그의 속성(사전형))
  ```python
    temp = soup.find('span', {'class': 'accent'})
    # 'span' 태그이면서 class가 accent인 케이스 찾음
    
    temp = soup.find('span')
    # 'span' 태그인 것을 찾음
  ```
  - find_all의 경우 여러개가 나오므로 꼭 한개씩 접근해야 함.
  
### 단일태그 찾은 경우
  - 값[속성명]으로 해당 속성 값을 가져올 수 있음.
  ```python
  atag = t.find('a')
  atag['title'] # 해당 a태그의 title 속성값을 가져옴.
  ```
  
  - get_text() 으로 사이에 있는 텍스트값을 가져올 수 있음.
  ```python
  ptag = t.find('p')
  ptag.get_text()
  # <p>aa</p>의 경우 aa를 가져옴.
  ```
