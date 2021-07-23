# Seeding

---

## Data Load

- Python [manage.py](http://manage.py) loaddata <seed_file>
    - seed_file에는 JSON, XML 사용 가능

## Data Dump

---

### json 형식 저장

- python [manage.py](http://manage.py) dumpdata 앱명 > 파일명.json : 그냥 저장

### 들여쓰기 하여 저장

- python [manage.py](http://manage.py) dumpdata 앱명 —indent=2 > 파일명.json

## JSON

---

```json
{
	"data":"value",
}

- JSON_Object["data"]로 접근

{
	"data":{
		"title":"title_data_01",
		"content": "content_data_01"
	}
}

- JSON_Object["data"]["title"]로 접근
```

## 대량의 시드 데이터 추가

---

### 1. 설치

```json
pip install django-seed
```

### 2. INSTALLED_APPS 등록

- INSTALLED_APPS 맨 아래에 'django_seed' 추가

### 3. 명령어 실행
- 앱 내의 모델에 맞추어서 데이터의 수만큼 임의의 데이터가 들어감
- 따로 유효성 체크는 하지 않으므로 유의
```json
python manage.py seed 앱명 --number=데이터수;
```

