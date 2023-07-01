
## INSTALL DOCKER

A very detailed instructions to install Docker are provide in the below link

https://docs.docker.com/get-docker/

For Demo, 

You can create an Ubuntu EC2 Instance on AWS and run the below commands to install docker.

```
sudo apt update
sudo apt install docker.io -y
```


### Start Docker and Grant Access

A very common mistake that many beginners do is, After they install docker using the sudo access, they miss the step to Start the Docker daemon and grant acess to the user they want to use to interact with docker and run docker commands.

Always ensure the docker daemon is up and running.

A easy way to verify your Docker installation is by running the below command

```
docker run hello-world
```

If the output says:

```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

This can mean two things, 
1. Docker deamon is not running.
2. Your user does not have access to run docker commands.


### Start Docker daemon

You use the below command to verify if the docker daemon is actually started and Active

```
sudo systemctl status docker
```

If you notice that the docker daemon is not running, you can start the daemon using the below command

```
sudo systemctl start docker
```


### Grant Access to your user to run docker commands

To grant access to your user to run the docker command, you should add the user to the Docker Linux group. Docker group is create by default when docker is installed.

```
sudo usermod -aG docker ubuntu
```

In the above command `ubuntu` is the name of the user, you can change the username appropriately.

**NOTE:** : You need to logout and login back for the changes to be reflected.


### Docker is Installed, up and running 🥳🥳

Use the same command again, to verify that docker is up and running.

```
docker run hello-world
```

Output should look like:

```
....
....
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
...
```


### Clone this repository and move to example folder

```
git clone https://github.com/iam-veeramalla/Docker-Zero-to-Hero
cd  examples
```

### Login to Docker [Create an account with https://hub.docker.com/]

```
docker login
```

```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: abhishekf5
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### Build your first Docker Image

You need to change the username accoringly in the below command

```
docker build -t asimsk757/Django .
```

Output of the above command

```
   Sending build context to Docker daemon  112.1kB
Step 1/7 : FROM ubuntu
 ---> 99284ca6cea0
Step 2/7 : WORKDIR /app
 ---> Using cache
 ---> 8abdfb5b709f
Step 3/7 : COPY requirements.txt /app
 ---> Using cache
 ---> 07e20d58f190
Step 4/7 : COPY devops /app
 ---> Using cache
 ---> 9b23ccca4968
Step 5/7 : RUN apt-get update &&     apt-get install -y python3 python3-pip &&     pip install -r requirements.txt &&     cd devops
 ---> Using cache
 ---> ba7ad7546864
Step 6/7 : ENTRYPOINT ["python3"]
 ---> Using cache
 ---> d017769c51f9
Step 7/7 : CMD ["manage.py", "runserver", "0.0.0.0:8000"]
 ---> Using cache
 ---> 1506ce93b188
Successfully built 1506ce93b188
```

### Verify Docker Image is created

```
docker images
```

Output 

```
REPOSITORY                         TAG       IMAGE ID       CREATED             SIZE
django                       latest    1506ce93b188   About an hour ago   503MB
asimsk757/my-first-docker-image    latest    aaa9e2a11335   15 hours ago        467MB
ubuntu                             latest    99284ca6cea0   3 weeks ago         77.8MB
hello-world                        latest    9c7a54a9a43c   8 weeks ago         13.3kBago    13.3kB
```

### Run your Django Docker Container

```
docker run -p 8000:8000 -it 1506ce93b188
```

Output

```
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
July 01, 2023 - 08:12:45
Django version 4.2.2, using settings 'devops.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
```

### Push the Image to DockerHub and share it with the world

```
docker push asimsk757/django
```

Output

```
Using default tag: latest
The push refers to repository [docker.io/asimsk757/django]
ec0dc19d4c1d: Pushed
d623d7ee67f0: Pushed
ed27b5889ede: Pushed
5c42c5d5c104: Mounted from asimsk757/my-first-docker-image
cdd7c7392317: Mounted from asimsk757/my-first-docker-image
latest: digest: sha256:7e9ce9fe9ad22dd07e26532161edad1ed0c2f48c2f436f02d8559ef414f60049 size: 1363
```
