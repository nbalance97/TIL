# 유닉스 커맨드



1. man 명령어

    ⇒ 명령어에 대한 정보, b : 뒤, space : 다음

2. 위치 관련 명령어
    1. ~ : 현재 사용자의 홈 디렉토리
    2. / : 최상위 디렉토리(루트)
    3. pwd : 현재 작업 디렉토리
    4. cd : 작업 디렉토리 변경
3. 경로
    - 절대 경로 : 루트 디렉토리를 기준 고유 경로
    - 상대 경로 : 나의 현재 위치를 기준으로 나타낸 경로
        - .. : 부모 디렉토리 
4. cd 명령어
    1. cd / : 루트
    2. cd ~ : 홈디렉토리
    3. cd - : 직전의 경로
5. ls 명령어
    - 현재 디렉토리 내의 폴더 / 자식 디렉토리 보여줌
    - ls -l : 결과를 긴 리스트 형식으로
    - ls -a : 숨김 파일 표시, .파일명 : 숨김 파일
    - ls 파일명 : 파일명에 대한 정보,
    - ls 디렉토리명 : 해당 디렉토리 내의 내용 정보
    - ls -d 디렉토리명 : 디렉토리 자체의 정보 보여줌
6. mkdir 
    - 디렉토리 생성
    - mkdir 디렉토리명 ⇒ 현재 디렉토리에 새로운 디렉토리 생성
7. touch
    - 파일 생성
    - touch 파일명 ⇒ 현재 디렉토리에 새로운 파일 생성
8. mv
    - mv 파일 디렉토리 : 파일을 디렉토리로 이동
    - mv 디렉토리 디렉토리 : 디렉토리의 이름 변경
    - mv 파일 파일 : 파일의 이름 변경
9. cp
    - cp 파일1 파일2 : 파일1의 복사본을 파일2로 만듬(덮어씌움)
    - cp -i 파일1 파일2 : 덮어씌울건지 물어봄
    - cp -r 디렉토리1 디렉토리2 : 디렉토리1의 복사본을 디렉토리2로 만듬.
    - 복사 붙여넣기 해서 손실되는 문제가 많으므로 i 꼭 써주기.
10. rm
    - 파일 / 디렉토리 삭제
    - rm 파일명
    - rm -r 디렉토리명
    - rm -i 디렉토리명 : 삭제하기 전에 확인(y/n)
11. cat
    - 파일들을 붙여서 출력
    - cat 파일명, [파일명]
12. less
    - less 파일명, [파일명]
    - 새로운 화면에 내용을 편리하게 보도록 출력
    - 하나의 파일씩 출력 (:n → 다음파일, :p → 이전파일)
    - space : 아래로 한페이지, b: 위로 한페이지, g : 처음, G : 맨 마지막
    - q : 종료

13. 파일 내용
    - head 파일명 : 처음 10줄만 출력
    - head -n 줄수 파일명 : 처음부터 입력한 줄수만큼 출력
    - tail 파일명 : 맨 마지막 10줄 출력
    - tail -n 줄수 파일명 : 맨 마지막에서부터 입력한 줄수만큼 출력