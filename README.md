# Assignment (Nginx, Docker, Nodejs)
## Verify candidate's ability to work with Jenkins
for more details : 

● Create a Dockerfile for Nodejs application.

~~~
FROM node:12.18.1
ENV NODE_ENV=production

WORKDIR /app

COPY ["package.json",  "./"]

RUN npm install --production

COPY . .

CMD [ "node", "server.js" ]
~~~

● Make the necessary changes in the code configuration.
(config/default.json)


● Create 3 different docker environments using docker-compose, each
service(Nodejs, Mongodb, Mysql) in the environment should run in a
separate container.
1. mango db databse Docker-compose file
 ~~~
 #nodjs

version: '3'

services:
  nodejs:
    build:
      context: .
      dockerfile: Dockerfile
    image: nodejs
    container_name: nodejs
    restart: unless-stopped
    env_file: .env
    environment:
      - MONGO_USERNAME= vishal
      - MONGO_PASSWORD= 123
      - MONGO_HOSTNAME= db
      - MONGO_PORT= '27017-27019'
      - MONGO_DB= mydb1
    ports:
      - "80:443"
    volumes:
      - .:/home/node/app
      - node_modules:/home/node/app/node_modules
    networks:
      - app-network
    command: ./wait-for.sh db:27017 -- /home/node/app/node_modules/.bin/nodemon app.js
volumes:
	- .
	- node_modules
 ~~~
2.  MYSQL DATABSE Docker-compose file
 ~~~
 # MYSQL db creating
version: '3'  
services:
  mydbos:
    image: mysql:5.7
    volumes:
	- mysql_storage:/var/lib/mysql
    restart: always
    environment:
	MYSQL_ROOT_PASSWORD: rootpass
	MYSQL_USER:vishal
	MYSQL_PASSWORD:123
	MYSQL_DATABASE: mydb

volumes:
    mysql_storage		
    
 ~~~
● Make sure all data will be persistent even if any of the containers crashes,
restarts.
~~~
volumes:
   - mysql_storage`
 ~~~  
● Make sure services will start in the right order (i.e. wait for mysql
initialization)
~~~
dependes on:
  - node.js
 ~~~
● Add a custom configuration to Nginx
○ Enable HTTPS and redirect HTTP to HTTPS
○ Create a self-signed certificate or use LetsEncrypt, also document
the commands you are using to create the certificate
~~~
certbot:
    image: certbot/certbot
    container_name: certbot
    volumes:
      - certbot-etc:/etc/letsencrypt
      - certbot-var:/var/lib/letsencrypt
      - web-root:/var/www/html
    depends_on:
      - webserver
  ~~~

# Conclusion:
     As mentioned inside the assignment we have created Dockerfile for NodeJS Application and have created three different enviornments for NodeJS, MongoDB & MySQL, also launched three different containers for each of them. We have created persistant storage so that it will be availble even if we restart the containers.
     
# Errors:
     As I have not worked on NodeJs applications earlier it is difficult for me to understand the code and what types of files are available there. I have tried my best to do this but unfortunately I am getting errors. These errors might be due to I didn't understand the code as there are multiple files in the document that you have shared through the link.

