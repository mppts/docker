FROM python:3
ENV PYTHONUNBUFFERED 1
RUN mkdir -p /code/
WORKDIR /code/
ADD requirements.txt /code/
RUN pip install -r requirements.txt
   
RUN  django-admin startproject app . && \
     python3 manage.py makemigrations && \
 	 python3 manage.py migrate && \
	 sed -i 's/ALLOWED_HOSTS = \[\]/ALLOWED_HOSTS = \["192.168.99.114" ,\]/' ./app/settings.py && \
	 echo "from django.contrib.auth.models import User; User.objects.create_superuser('admin', 'admin@example.com', 'pass')" | python3 manage.py shell


ADD . /code/


ENTRYPOINT python3 manage.py runserver 0.0.0.0:8000
