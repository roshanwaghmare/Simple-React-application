# Simple-React-application
Docker 

## Creating the Dev Dockerfile

- now we have to name file inside frontend so 

````
ls
cd frontend
code .

in vs code create docker file
Dockerfile.dev

````

- now we name the file docker.dev so

````
 docker build .                      #will no work
docker build -f Dockerfile.dev .    # -f means we are spicifing file to be build 
  
````

- we have removed the duplicate wo copies of dependencies, and we really do not need two.The easiest solution here is to delete the "node modules" folder inside of our working directory .which I'm gonna do right now.so I'll delete that entire folder by moving it to trash.

````
docker run e79c31d7f07d249031b1dfa08458dcb5a673eb96de07595a0a5c0a4 container id
- Now if we run the contianer it will not work we need to open or expose the port

docker run -p 3000:3000 e79c31d7f07d249031b1dfa08458dcb5a673eb96de07595 container id  
````
web localhost:3000
