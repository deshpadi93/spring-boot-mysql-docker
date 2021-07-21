
**Dockerising Spring Boot app with MySQL**

Install Docker for from the official website [https://hub.docker.com/](https://hub.docker.com/)

 - After installation in order to check whether the Docker is running or
   not

***docker   –v*** (current Docker version) 

Then open the “**DockerFile**” from the project folder
This contains the commands for generating Docker image

    FROM openjdk:8  
    ADD target/demo-docker.jar demo-docker.jar  
    EXPOSE 8080  
    ENTRYPOINT ["java", "-jar" , "demo-docker.jar" ]

**FROM** will pull openjdk from Docker repo.

**ADD** will copy build jar form target folder to root folder of the Docker.

**EXPOSE** will expose the port 8080.

**ENTRYPOINT** will execute the command given.

`docker build -f Dockerfile -t docker-spring-boot-user .`
–f <FILENAME> -t <tag-name-for-Docker-image> and **‘.’** is the file path that is current as of now.

    docker image ls

This command will give you list of images you have and you can find one with the name **docker-spring-boot-user.**

**Docker Setup for MySQL**

Create and run an image of the MySQL database.

    docker run -d  -p  6033:3306 --name=mysql-docker --env="MYSQL_ROOT_PASSWORD=root"  --env="MYSQL_PASSWORD=root"  --env="MYSQL_DATABASE=demo" mysql

To check this, we can run.

 `docker container ls` 

Now we can check by logging in to MySQL.

    docker exec -it mysql-docker bash
*(mysql-docker is the tag name we have given while creating)*

Import the sql by following command

    docker exec -i docker-mysql mysql -uroot -proot demo <demo.sql

**Run application inside the Docker**

`docker run -t --link mysql-docker:mysql -p 8080:8080 docker-spring-boot-user`

**--link** will link the MySQL container and will be exposing the port 8080

Now it will open up in

    http://localhost:8080/spring/user

POST 

    {
    "username":"test"
    }
