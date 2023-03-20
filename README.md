# App to create courses and sell for students

This app allow a teacher to create question for students test them skills.

## Command to init the Docker

```bash
sudo docker-compose build
sudo docker-compose up -d
```

### To stop the docker
```bash
sudo docker stop mongo         
sudo docker stop node-courses  
sudo docker stop angular-courses
```

### To restart
```bash
sudo docker-compose down     
sudo docker-compose build
sudo docker-compose up -d
```

# Angular app

If you want to deploy it in a server you need to change the Dockerfile with the instructions in the repository.
And add this to the docker compose:
```yml
    angular:
        build:
            context: ./angular-question
        container_name: angular-question
        restart: always
        ports:
            - "4200:4200"
        volumes:
            - ./angular-question:/app
        working_dir: /app
        environment:
            - NODE_ENV=development
        networks:
            - course-project
 ```

With this configuration you need to entry in the folder of Angular and make the command to start a serve:
```bash
sudo ng serve
```

# Nodejs

You need to create the conn.js as the documentation.
You can change it as you want. But, save the user and password that you use to connect in the database.
If you have proble, try to do npm install in the local folder

# MongoDB

This docker create the DB but dont create the user that is needed to run the app properly

To do it follow this steps:

## Download Mongo commands in your system
You need to have the command "mongo" to create a User.

Alternately you can use some GUI to do it. Like a Robo3T o Mongo Compass.
In this case, follow the documentation of your GUI software.


### Fedora
```bash
sudo nano /etc/yum.repos.d/mongodb.repo 
```
It will open a editor in your cmd. Type this code in it.

```bash
[Mongodb]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/8/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
```

To be sure and for you security, update the system
```bash
sudo dnf update -y 
```
And then install mongo
```bash
sudo yum install -y mongodb-org
```

### Other OS
Follow the documetation https://www.mongodb.com/docs/manual/installation/
Notice that you just need the mongodb-org and you dont need to install the server.
The Docker will be your server.

## Create the user
Entry in the mongo.
```bash
mongo --port 27017 -u admin -p '3^9r4$f$o7k*TSk9rJBWbh' --authenticationDatabase 'admin'
```
or
```bash
mongo -u admin -p 3^9r4$f$o7k*TSk9rJBWbh --authenticationDatabase admin
```

Entry in your collection, if you dont change it is the courses:
```bash
use courses
```
Entry the user that you use in Nodejs.
Change the user and the password

```bash
db.createUser({
    user: "{user}",
    pwd: "{password}",
    roles: [
        { role: "readWrite", db: "courses" }
    ]
})
```
example:
```bash
db.createUser({
    user: "auth",
    pwd: "3^9r4$f$o7k*TSk9rJBWbh",
    roles: [
        { role: "readWrite", db: "courses" }
    ]
})
```


