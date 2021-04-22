Django에 대한 모든 것
==================

### [Django 공부 자료 :https://tutorial.djangogirls.org/ko/ 와 https://www.djangoproject.com/]

#### Django-admin page의 계획

      # 먼저 Backend에서 우리는 어떻게 다룰 것인가?
      1. Machine Learning을 돌리는 부분은 Django&AWS&MySQL을 활용해야함
         => Django 모델 생성은 여기서만 하자. 
      2. 일반적인 정보들은 React&Firebase를 활용해야함[React README.md 참고]
         => 여기서는 Django 모델을 생성하지 말자.
      3. 관리자페이지를 연동함에 즉, Frontend, Backend 연결 부분은 React&Django를 연결해야함
      
            cf) 어떤 부분이 어려워??
            - Django DB를 Firebase와 MySQL 둘다 어떻게 연결해??
              => 일반적인 부분은 Firebase로만 연결하고 증명과정은 MySQL을 활용하자
              => but settings.py에서 DATABASES는 어떻게 연결해야 할까?
              => Firebase를 Django와 연결하지 말고 React와 연결하자
              => 이제 시도해보자
      
      # 개발순서
      1. React-FirebaseStore 연결 :  https://velog.io/@chdb57/%E3%85%87-bvk60v5ufy
      2. DjangoAdmin-React 연결
      3. Django Model 생성 및 Machine Learning 코드 연결

#### Django의 시작 

      # mac에서 장고 시작
      $ mkdir djangogirls
      $ cd djangogirls
      $ python3 -m venv myvenv
      
      # 가상환경 활성화!!!!! 매우 중요 모든 과정에서 항상 활성화되어야지 DB에도 접근을 할 
      $ source myvenv/bin/activate
      
      # 최신버전인지 확인
      (myvenv) ~$ python3 -m pip install --upgrade pip
      
      # 최신버전 장고 설치
      $ pip install Django==3.0.8
      $ pip install djangorestframework
      
      # 장고프로젝트 시작하기
      $ django-admin startproject mysite(다른 이름 써도 됨).(점 필수)
      
      # 설정변경
      시간, 경로, 서버 설정함.
      $ 시간 : "Asia/Seoul", 
      $ 경로 : STATIC_URL = os.path.join(BASE_DIR, "/static/") - 오류나면 import os 함
      $ python3 manage.py migrate를 함
      $ python3 manage.py runserver를 돌려서 서버가 잘 작동하는지 봐야함
      
      # 앱 시작하기
      python manage.py startapp myapp(아무 이름으로 설정하면 됨) or django-admin startapp myapp
      python manage.py migrations (이름) 이렇게 하면 데이터베이스와 앱이 연결이 됨
      
      # Model 생성과 활성화
      models.py에서 class 모델을 만들고 settings.py에서 polls.apps.PollsConfig를 넣었음
      admin.py에서 models.py에서 생성된 모델을 admin.site.register(Model)을 사용하면 됨
      $ python3 manage.py makemigrations polls 그러면 model들이 생성됨
      $ python3 manage.py sqlmigrate polls 0001 이러면 내가 생성한 모델들의 형태들이 보여짐
      $ python3 manage.py migrate 이제 설정을 바꿀 때마다 습관적으로 하게 되는듯
      
      # Admin Page
      생성 : $ python3 manage.py createsuperuser
      실행 : $ python3 manage.py runserver
      
#### 데이터베이스설정 : https://pypi.org/project/mysqlclient/

      나는 mysql을 사용하려고 하므로 mysql client를 사용함
      먼저, python 가상환경이 활성화되어 있는지 봐야함 그 다음 아래의 코드를 입력함
      $ brew install mysql
      $ pip install mysqlclient
      밑에 [MySQL 설치 여정]을 먼저 한다.
      $ mysql.server start 서버 실행
      $ mysql -u root -p 입력 후 비밀번호 접속
      $ mysql> CREATE DATABASE <dbname> CHARACTER SET utf8;
      $ mysql> use <dbname>
      $ mysql> show tables; 이러면 table에 어떤 데이터가 있는지 알 수 있음
      
      # MySQL과 Django 연결하기
      $ touch my_settings.py 를 사용해 데이터베이스 세팅을 둘 파일을 만든다.
      아래와 같은 데이터베이스 설정을 해준다.
      DATABASES = {
          'default' : {
              'ENGINE': 'django.db.backends.mysql',    [1] : 사용할 엔진 설정. 그대로 두면 됨
              'NAME': 'django_pollsdb',                [2] : 연동할 MySQL의 데이터베이스 이름
              'USER': 'root',                          [3] : DB 접속 계정명
              'PASSWORD': 'password',                  [4] : 해당 DB 접속 계정 비밀번호
              'HOST': 'localhost',                     [5] : 실제 DB 주소, 따로 설정 안했으면 그대로 두면 됨
              'PORT': '3306',                          [6] : 포트번호, 따로 설정 안했으면 그대로 두면 됨
              'OPTIONS': {                             [7] : 이 경우 strict mode 상에서 오류가 발생했을 때 넣었음 : https://zetawiki.com/wiki/Django_mysql-sql-mode_warning
                  'init_command': 'SET sql_mode="STRICT_TRANS_TABLES"'
              }
          }
      }
      그리고 기존에 setting.py는 다음과 같이 수정함
      import my_settings
      DATABASES = my_settings.DATABASES
      $ python manage.py migrate 실행 그러면 목록들이 나온다.
      
#### Django와 React 연결하기 : https://www.valentinog.com/blog/drf/#django-rest-with-react-setting-up-a-python-virtual-environment-and-the-project

      # 가상환경실행 및 설치
      source myvenv/bin/activate
      
      
#### [Django로 협업하기]
      
      1. pip freeze > requirements.txt 
      - pip freeze : 현재 환경에서 설치한 패키지를 알려주는 명령어
      - requirements.txt : Django Project에 생성해놓은 text file
      - pip freeze > requirements.txt를 사용하면 requirements.txt에다가 자동으로 설치한 패키지를 알려줌
      
      
#### [MySQL 설치 여정]

- mac에서 MySQL 설치를 다루고자 한다. 너무 많은 요소들이 있어서 설치하는데만 1시간 쓴듯

- 참고 링크 : https://velog.io/@max9106/mac%EC%97%90-MySQL-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-4ck17gzjk3

- mysql 사용자추가/DB 생성/권한부여 : https://nickjoit.tistory.com/144

      MySQL 설치
      $ brew update 최신 버전
      $ brew search mysql mysql 버전 확인
      $ brew install mysql 설치
      
      MySQL 설정
      $ mysql.server start 서버 실행
      $ mysql.secure_installation 설정으로 넘어가기
      1. 복잡한 or 쉬운 비밀번호
      2. 접속시 -u 필요 or 불필요
      3. 원격접속 불가능 or 가능
      4. 데이터베이스 제거 or 유지
      5. 변경된 권한을 데이블에 적용 or 미적용
      
      $ mysql -u root -p 입력 후 비밀번호 접속
      로그아웃 시: exit 또는 quit
      MySQL 서버 종료: mysql.server stop
      
      MySQL 삭제
      $ sudo rm -rf /usr/local/var/mysql
      $ sudo rm -rf /usr/local/bin/mysql*
      $ sudo rm -rf /usr/local/Cellar.mysql

#### [Django 유용한 정보]

      1. 장고 ORM으로 CRUD 다루기 : https://velog.io/@magnoliarfsit/ReDjango-4.-%EC%9E%A5%EA%B3%A0-ORM%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%84%9C-DB-CRUD-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0
      2. python 파일로 장고 제어 중 크롤링 사례 : https://beomi.github.io/2017/03/01/HowToMakeWebCrawler-Save-with-Django/
      3. MySQL 같은 경우 이모티콘을 저장할 때 bit 차이로 문제가 발생 : https://velog.io/@devmin/mysql-database-utf8mb4-django

#### [Django Rest Framework] : https://www.django-rest-framework.org/
      
      1. pip install djangorestframework -> settings.py에서 "rest_framework"를 추가
      2. 인증 및 권한 설정 
         GET과 POST 방법에서 모든 사용자에게는 GET, 인증된 사용자만 POST 사용 가능
         IsAuthenticatedOrReadOnly : 인증된 사용자에게는 허용하고 그렇지 않으면 읽기만 가능
         IsAuthenticated : 인증된 사용자에게만 허용 <- Mypage에서는 허용
         IsAdminUser : 관리자에게만 허용
         AllowAny : 모두 허용
