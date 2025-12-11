FROM python:3.12-slim

WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential netcat-openbsd \
 && rm -rf /var/lib/apt/lists/*

COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

COPY . /app

CMD bash -lc '\
  echo "Waiting for $PG_HOST:$PG_PORT..." && \
  until nc -z $PG_HOST $PG_PORT; do sleep 0.5; done; \
  echo "Postgres is up."; \
  python manage.py migrate && \
  gunicorn pts_backend.wsgi:application --bind 0.0.0.0:8000 \
'
