FROM python:3.6-alpine

ENV admin_user="admin"
ENV admin_email='optional'
ENV admin_pass='password'

RUN apk add --no-cache postgresql-libs && \
    apk add --no-cache --virtual .build-deps gcc git musl-dev postgresql-dev && \
    git clone https://github.com/chakki-works/doccano.git && \
    cd doccano && \
    python3 -m pip install -r requirements.txt --no-cache-dir && \ 
    cd app && \
    python manage.py migrate &&\
    echo "from django.contrib.auth import get_user_model; \
        User = get_user_model(); \
        User.objects.create_superuser('$admin_user', '$admin_email', '$admin_pass')" \
        | python manage.py shell && \
    apk --purge del .build-deps

WORKDIR /doccano/app

CMD python manage.py runserver  0.0.0.0:8000
