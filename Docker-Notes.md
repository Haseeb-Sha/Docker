#-------------Docker------------
dockerhub
- docker run -e (env variable)
  docker run -e POSTGRES_PASSWORD=haseeb -p 1001:5432(out/inv) postgres:13.10 
- docker ps -a  # all stopped containers
- docker ps     # running containers
- docker pull redis:latest
- docker run -d container_id or --name test redis:6.2     # runs in background
- docker restart container_id
- docker rmi image_name or container_id
- docker start container_id
- docker inspect container_id
- docker logs container_id
- docker exec -it container_id /bin/bash and use env 
- docker start container_id 					( working with container not images)
# USER NODE MONGODB AND MONOGEXPRESS
- docker pull mongo 
- docker pull mongo-express
- docker network ls
- docker network create mongo-network
- docker run -p 27017:27017 -d  ... --network mongo-network -e --name mongod-db mongo
- docker run -d -p 8081:8081 -e ..... --net mongo-network --name mongo-express  mongo-express
- docker logs -f
# Docker Compose
-> create a file name.yaml

version: '3' 
servives:
	mongodb:         (name of the container --name)
		image: mongo
		restart: always
		ports:
		-
		-
		environment:
		- 
		-
		volumes:
		- db-data:/var/lib/mysql/data
volumes:
	db-data:
		driver: local
		
-> network is not needed here because docker compose does that on it's own
-> use image name to connect to each other
- docker-compose -f mongo.yaml up or down
# Buidling Custom Images
-> create a Dockerfile
FROM node      (already node image instead of alpine)
ENV 
RUN            (run any linux commands,build time setup)
COPY           ( execute on the host that means from source-host to container)
WORKDIR			(set defualt home dir)
CMD 			( can be overidden via cmd line args)
RUN
ENTRYPOINT 	(wrapper script does at runtime not overiden)
EXPOSE

- docker build -t myapp:1.0 . 
- docker run myapp:1.0

# Docker Registry
- ECR (each image has one image , but with different tags)
- docker login ip:port
- docker tag name:tag aw..../my-app:1.0
- docker push registryDomain/imageName:tags
- cat ~/.docker/config.json (contains all tokens for login auth)
- /etc/docker/daemon.json (for adding insecure resgistories)

# Docker Volumes
- docker run -v hostdir:containerdir  (host volume)
- docker run -v path 		(anonymous volume) --> /var/lib/docker/Volumes 
- docker run -v name:path (datapath)	(named-volumes) (production grade)
- docker volume ls
- docker inspect volume_typess

# best docker practice
- use official image (like node instead of unbuntu)
- use FROM node:20.0.2 (use versions)
- don't use full blown os images ( like ubuntu because of unecessary packages and security issues e.g alpine)
- optimize image layer caching ( docker history myapp:1.0)
- create .dockerignore file and remove necessary files
- create build stage and runtime stage ( use multi-stage built)
   # Build Stage
   FROM maven AS build
   WORKDIR /app
   COPY myapp /app
   RUN mvn package
   # Run stage
   FROM tomcat
   COPY --from=build /app/target/file.war /ur/local/wa...
- less layering and improved caching
- use dedicate user and grou for image (RUN and USER)
- docker scout cves (image name) scanning images
   
   
   

