FROM python:3.9-alpine

ADD ui /app/ui
ADD requirements.txt /app
RUN pip install -r /app/requirements.txt

ENV MONGO "172.17.0.2"
ENV MONGO_PORT "27017"
ENV FLASK_APP "ui.py"

ARG EXTERNAL_ACCESS ""

WORKDIR /app/ui
EXPOSE 8000

ENTRYPOINT gunicorn ui:app -b ${EXTERNAL_ACCESS:-0.0.0.0}
