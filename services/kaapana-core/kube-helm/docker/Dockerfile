FROM python:3.9-alpine3.12

LABEL IMAGE="kube-helm"
LABEL VERSION="0.1.0"
LABEL CI_IGNORE="False"

WORKDIR /app

RUN apk update && apk add postgresql-dev gcc python3-dev musl-dev
RUN apk add findutils
RUN apk add jq
RUN pip install psycopg2

ENV KUBE_LATEST_VERSION="v1.19.2"
ENV HELM_VERSION="v3.3.4"

RUN apk add --no-cache ca-certificates bash git openssh curl \
    && curl https://storage.googleapis.com/kubernetes-release/release/${KUBE_LATEST_VERSION}/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl \
    && curl https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz | tar -xzO linux-amd64/helm > /usr/local/bin/helm \
    && chmod +x /usr/local/bin/helm \
    && chmod g+rwx /root \
    && mkdir /config \
    && chmod g+rwx /config    
    
COPY files/requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY files/ ./
RUN chmod +x boot.sh

ENV FLASK_APP run.py

EXPOSE 5000

CMD ["./boot.sh"]
