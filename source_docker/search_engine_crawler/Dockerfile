FROM python:3.9-alpine

ADD crawler /app/crawler 
ADD requirements.txt /app
RUN pip install -r /app/requirements.txt 

ENV MONGO "172.17.0.2"
ENV MONGO_PORT "27017"
ENV RMQ_HOST "172.17.0.3"
ENV RMQ_QUEUE "test-rabbit"
ENV RMQ_USERNAME "user"
ENV RMQ_PASSWORD "user"
ENV CHECK_INTERVAL "60"
ENV EXCLUDE_URLS ".*github.com"

ARG DEFAULT_URL "https://vitkhab.github.io/search_engine_test_site/"

WORKDIR /app/crawler
EXPOSE 8000

ENTRYPOINT ["python", "-u", "crawler.py"]
CMD ["$DEFAULT_URL"]
