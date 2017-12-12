# SuperheroAPI
A sample python API that reveals superheroes' secret identities.

# Exposed enspoints
Endpoint  | Operations | Description
------------- | ----------------|----------
/api/v1/superhero/<superhero> | GET | Retrieve a specific superhero with the given name
/api/v1/superhero/<name> | POST | Add a new Superhero
/api/v1/superheroes | GET | Get All superheros

## Environment variables
Env Var  | Description | Default
------------- | ------------- | -------------
PORT | PORT number used by the API to listen to incoming request | 8080

# How to run th app locally
Note: all commands should run from the root directory of you application i.e: <your-code-full-path>/SuperheroAPI

## Pre-requisite to running it locally (first time only)
`pip install virtualenv`

`virtualenv venv`

## Install dependecies
`source venv/bin/activate`

`pip install -r requirements.txt`

## Start the server
`python server.py`

Check the applications is runing by typeing the following in your browser:
 `http://127.0.0.1:8080/api/v1/superhero/batman`


# Dockerisation

1- To dockerise your api first build the docker image using the 'DockerFile' (do not forget the '.' at the end)
`sudo docker build -t superhero-api .`

2- You should be able to find two images (python3 and superhero-api) in your local docker repo when running the following command: 
`sudo docker images`

Note that "pyhton3" is the base image you are building your container against

3- Now run the container 
`sudo docker run --rm -d -p 8080:8080 --name superhero superhero-api:latest`

To check your running docker instances run the following command 
`sudo docker ps`

To stop your running instance
`sudo docker stop superhero`

4- The API reads the Superheroes information from a txt file saved in the `data` folder. If you want to mount that file to the docker container so you can change the content with having to rebuild your image:

`sudo docker run --rm -d -p 8080:8080 --name superhero -v /home/ubuntu/code/SuperheroAPI/data:/data superhero-api:latest`

Note that you have to enter the source directory in the above command