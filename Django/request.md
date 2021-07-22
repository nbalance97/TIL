### request.GET 메소드로 정보 얻어올때

---

- request.GET['aa'] ⇒ 없으면 곧바로 keyerror가 뜬다. (딕셔너리와 같음)
- 해결책으로 request.GET.get('aa')를 해주면 없을떄 None이 나타나게 되어 처리해주면 된다.
