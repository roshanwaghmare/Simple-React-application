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

- Add more integrations
