# Using MySQL Database 

## 0. Intro

기본 제공하는 SQLite가 아닌 MySQL을 데이터베이스로 연동하면서 겪었던 삽질에 대한 내용이다.
AWS에서 제공하는 RDS를 사용하지 않고, OSX 환경에서 구성하였다. 

## 1. Install MySQL & Settings

1.1 brew를 통한 MySQL을 설치한다.

<pre><code>brew install mysql</code></pre>

1.2 root 비밀번호 설정한다.

<pre><code> mysql_secure_installation </code></pre>
	* Would you like to setup VALIDATE PASSWORD plugin? 
		- yes 해서 LOW를 선택했다.
	* Remove anonymous users?
		- 잘 모르겠어서 일단 yes
	* Disallow root login remotely?
		- 원격 접속은 해야지.. no
	* Remove test database and access to it?
		- yes
	* Reload privilege tables now? 
		- yes

1.3 Terminal 상에서 mysql 접속한다.

<pre><code> mysql -u root -p </code></pre>

## 2. Make DataBase 

2.1 mysql 접속 상태에서 데이터베이스를 생성한다.

test는 예시를 위하여 작성하였다, 원하는 이름으로 Database를 생성하면 된다. 

<pre><code> CREATE DATABASE test; </code></pre>


2.2 DATABASE 를 확인하기 위하여 아래의 명령어 사용한다.

<pre><code> SHOW DATABASES LIKE 'test'; </code></pre>

## 3. Django + MySQL

3.1 settings.py 에서 MySQL 데이터 베이스를 연동한다. 

<pre><code> vim settings.py </code></pre>


<pre><code> 
	DATABASES = {
		'default' : {
			'ENGINE': 'django.db.backends.mysql',
			'NAME' : 'test',
			'USER' : 'root',
			'PASSWORD' : '12345',  <- MySQL root계정에서 설정한 비밀번호 사용
			'HOST' : '127.0.0.1',
			'PORT' : '3306',
		}
	}
</pre></code>

3.2 migrate로 settings의 변경사항을 적용시킨다.

<pre><code> python manage.py migrate </code></pre>

3.3 runserver 이후 접속이 잘 되는지 확인한다. 

<pre><code> python manage.py runserver 8000 </code></pre>

