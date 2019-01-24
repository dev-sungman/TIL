# Django Project. 

## 0. Intro

AWS 에서 EC2 Instance를 생성한 이후의 Djagno , python3 설치에 관하여 정리를 하려고 한다. 
EC2는 Ubuntu 18.04 를 사용하였으며, 별도의 가상환경을 생성하지 않고 Native 환경에 패키지를 설치하였다. 

## 1. Install Packages

1.1 파이썬을 설치해준다.  

<pre><code>sudo apt-get install python3</code></pre>

1.2 Django 설치

<pre><code> pip3 install django </code></pre>

## 2. Make Project 

2.1 Project를 생성할 디렉터리 생성

<pre><code> mkdir djangoproj </code></pre>

<pre><code> cd djangoproj </code></pre>

2.2 Project 생성

<pre><code> django-admin startproject mysite </code></pre>

2.3 Polls 생성
manage.py 가 있는 경로에서 실행해 주어야 함. 

<pre><code> python3 manage.py startapp polls </code></pre>

## 3. Change Settings

<pre><code> vim settings.py </code></pre>

* ALLOWD_HOSTS 항목에서 허용할 IP를 설정해야한다. 이 글에서는 AWS EC2 Instance 에서 Django 서버를 구동하고
로컬에서 Django 서버를 접속하는 것을 가정으로 하기 때문에, 허용할 IP에 EC2 Instance의 IPv4 IP를 적어준다. 

* INSTALLED_APPS 에 'polls.apps.PollsConfig'를 추가해준다. 

* DATABASES는 필요에의해 변경하면 된다. 

* TIME_ZONE 은 'Asia/Seoul' 을 입력해준다.

## 4. Start 

4.1 서버를 실행할 때 python3 manage.py runserver 'IP':'Port' 로 실행이 가능하다. 

4.2 보안상의 문제를 위하여 Port를 변경하고 싶을 때는, 원하는 포트로 서버를 실행하고 EC2 Console 에서 Inbound 규칙을 설정해주어야 한다. 
	* Security - Inbound - CustomTCP 에서 Open을 원하는 Port를 입력해준다. 

4.3 성공적으로 Django 서버를 실행하였다면, 로컬 PC에서 'EC2 IP : 오픈한 Port' 번호로 접속해보자

example  
<pre><code> 123.45.6.7:8910 </code></pre>
