# Deploying Flask App Using Docker


### Table of Content

1-  Overview
2- Getting Started
- Prerequisites
- Steps


## Overview

This Repo presents the steps needed in order to deploy Flask application using Docker. Using Docker you won't need to add the dependencies on your machine or do any configuration. Everything is automated and isolated from the rest of your machine. Docker runs as a separate process on your machine.

## Getting Started

### Prerequisites : 
The only two prerequisites is to install docker engine and composer on your machine. Please follw the next steps according to your OS.
##### Docker Enginge : 
- For windows users:  click [here](https://docs.docker.com/desktop/windows/install/ "here")
- For linux users : click [here](https://docs.docker.com/engine/install/ubuntu/ "here").

##### Docker Composer :
you can find the required steps for both linux and windows on the following [link](https://docs.docker.com/compose/install/ "link"). 

##### Docker Linter:
For this repo I used vscode and installed docker extenstion as [Linter](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker "Linter") for formatting.

### Getting Started : 

1- Create new folder and add your flaskapp.py file inside that folder. 
2- Create new file inside the same folder called requirements.txt in order to add the modules and dependencies required for the application.

    flask
    pytz
    datetime
3- Create docker file named Dockerfile and add the following conent. 

    # build and image starting with pythom 3.8 which is the version needed for our app
    FROM python:3.8-slim-buster
    # set the working directory
    WORKDIR /code
    #set the variable used by flask command
    ENV FLASK_APP=flaskapp.py
    ENV FLASK_RUN_HOST=0.0.0.0
    #copy and install the depndeincies in the file requirments.txt
    COPY requirements.txt requirements.txt
    RUN pip install -r requirements.txt
    #set the port for the container to be 5000
    EXPOSE 5000
    #copy the current files to the file 
    COPY . .
    #set the default command for the container to flask run 
    CMD ["flask", "run"]
    
4- Add docker-compose.yml file in order to define the running services for the container.

    version: "3.9"
    services:
      web:
        build: .
        ports:
          - "5000:5000"
      redis:
        image: "redis:alpine"

5- Build your app using compose 
` docker-compose up`

6- Open your browser and type http://localhost:5000/. You should see the time of moscow displayed on your browser screen.
