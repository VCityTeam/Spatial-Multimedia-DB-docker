FROM python:3.6

LABEL maintainer "VCityTeam"
LABEL source.repo "https://github.com/VCityTeam/Spatial-Multimedia-DB-docker.git"

RUN apt-get update

ARG SMDB=/Spatial-Multimedia-DB

RUN apt-get install -y git

RUN git clone https://github.com/VCityTeam/Spatial-Multimedia-DB.git

WORKDIR /Spatial-Multimedia-DB

RUN pip install -r requirements.txt

ENV PYTHONPATH $SMDB

CMD ["python", "api/web_api.py"]

