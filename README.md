# Docker CyberLab

The name is suggestive — it gives a general idea, but let me dive deeper.

The main goal of this project is to expand my understanding of Docker, improve my Git skills, and explore basic web vulnerabilities in a hands-on way. I’ll also document the process and explain what I learn — partly for others, but mostly for myself.

As the assembly creator once said:  
> “If you want to make a pie from scratch, you must first invent the universe.”  

So that’s what I’m trying to do here — building from scratch and throwing myself into the unknown. Jumping into the ocean without knowing how to swim.

### Objectives:
- Use Docker to create virtual environments
- Understanding the enemie
- Practice hacking techniques and document the process

## Part 1: Docker

First I installed Docker engine and make sure that Docker compose was too installed.

### My definitions:
- Docker is a Software that lets you create conntainers. These containers are simmilar to Virtual Machines but without the GUI, full operating systems functionalities and other things that are user-like. This makes the creation of enviroments easy and fast to work on.
- Docker Compose its a plugin that resolves a big headache that is the inter-correlation of containers. It has a file called docker-compose.yml that allows to create a mini- infrastructure. 

Having the "theoric" base line setted up now we can build the structure.
A directory was needed to all services that were to be used. So a directory called attacker/, db/ and web/ were created. This folders will have something in the future.

### For now the structure of the file is like this:
````
docker-cyberlab/
├── docker-compose.yml
├── Juice-Shop/
├── attacker/
│   └── Dockerfile
├── README.md
````

### Some necessary insights.

The Juice-Shop directory is empty and doesnt need to be there. It only is for visual understanding of the tecnologies in use. In order to build a container there is the need to have a DockerFile, this DockerFile has the basic set up needed to intialize an image and basic function if wanted. This DockerFile is called when the docker-compose.yml is running. The fact is, the web page being used is a prefabricated one called OWasp Juice Shop, that being said, instead of having the DockerFile of the web page there is a reference to the docker repository where the web app is pulled. The same happens with the database. The init.sql file is needed to start the db but for now is empty. 

### This is the docker-compose.yml code:

````
version: '3'
services:
  juice-shop:
    image: bkimminich/juice-shop
    container_name: juice-shop
    ports:
      - "3000:3000"
    restart: always

  attacker:
    container_name: juice-attacker
    build: ./attacker
    tty: true
````
### Has I said:
- juice-shop: -> image: bkimminich/juice-shop // image: pulls a already created one
- attacker: -> build: ./attacker // finds and loads the DockerFile on the directory

### DockerFile of the attacker container:

````
FROM kalilinux/kali-rolling

RUN apt-get update && apt-get install -y \
    nmap \
    net-tools \
    iputils-ping

CMD ["/bin/bash"]
````
This simple definition gets kali linux (with FROM), installs some basic tools (with RUN) and starts the /bin/bash on the container (with CMD).

