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

## Docker Volumes

To be able to update the src code we have to make configration so every time when we chnage the src code we dont have to rebuild the image

With a Docker volume, we're essentially setting up a **mapping** from a **folder inside the container** to a folder **outside the container**. Now you might be wondering, why did we not just make use of volumes before if this kind of solves the issue.


to run docker volume we have to execute long cmds 

1) -v Means volume $(pwd)  we're essentially saying get the present working directory or the path to it, and take this folder, like this front end folder and everything inside of it, and map it up to the app folder

2) earlier we have deleted node modules folder hence this 1 is not working hence we are provide path

3) when we use {:} colun that mean copy the path and paste it to now we dont want to copy we just want to execute
 
````
docker run -p 3000:3000 -v $(pwd):/app 5906c40a635ff25c69a33c239195db9eca6ae7788a46175798c9cf3  #this cmd will not work

docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app 5906c40a635ff25c69a33c239195db9eca6ae7788a46175798c9cf3 this is not working too its not good way it use long cmd  
````

4) with the help of docker compse we can execute multiple cmds
   
## docker-compse.yml

````
docker-compose up
````
5) now we are able make  chnages on src code without building contianer  again.

## Executing Tests

````
docker build -f Dockerfile.dev

docker run c2e9788812f39f7120abf76e7de077c969b3 npm run test  
#When we run Docker run by default we get a connection to standard out inside the container.

So will use -it flag
docker run -it c2e9788812f39f7120abf76e7de077c969b3 npm run test

now with nice view we can now able to execute

````

copy and pastes the same two times 

````

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
````
now we made changes in app.test.js and hit enter it still getting only 1 test 

We've got a container that's been created specifically to run some tests. When we created that container, we essentially took a snapshot of all of our working files and folders, and put that inside the container. So, this very temporary container that we've made just to run our tests, does not have all that volume stuff set up. That is the issue. And so, without any of those volumes set up, we are using old and outdated files inside of our container.





