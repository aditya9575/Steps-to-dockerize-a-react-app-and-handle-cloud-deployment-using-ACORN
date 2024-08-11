# Steps-to-dockerize-a-react-app-and-handle-cloud-deployment-using-ACORN--->
Docker Notes in context of react app 

A)install Docker -> Docker Desktop For Windows 
B) to check if its successfully installed -> docker --version 
C)Now to containerize an app into docker we do the following --->

1- Create a new file in the root folder -> example name Dockerfile
And this docker file will contain steps we need to perform to run this application 

2- as we need node so search for which node version is there that you are using by the command -> node -v 
now go to -> docker node image 
and from here select a node image preferably a light weighted node image but you can select accordingly example 20-alpine AND SPECIFY IT IN OUR DOCKER FILE

3- NOW WE CREATE A APP FOLDER INSIDE OF OUR WOKING DIRECTORY 

4- NOW WE WILL COPY OUR PACKAGE.JSON FILE TO->DOT->  "." THAT MEANS ITS BEING COPIED IN OUR WORKING DIRECTORY 

5- NOW WE WILL RUN npm install- WHICH WILL INSTALL ALL THE DEPENDENCIES INSIDE OF OUR PACKAGE.JSON FILE 

6- NOW WE TAKE EVERYTHING INSIDE OF IT AND COPY IT IN THE WORKING DIRECORY -> COPY . . 

7- BUT WE WANT TO EXCLUDE THE NODEMODULES FOLDER SO WE CREATE A .DOCKERIGNORE FILE 

8- inside the DOCKERIGNORE FILE -> 
 node_modules

9- NOW IN THE DOCKER FILE -> EXPOSE {THE PORT NAME}

10- NOW IN THE DOCKERFILE -> CMD[ "npm" , "run" , "dev" ]

11- In our package.json after the vite script we need to add ->
--host 0.0.0.0, 
(so that it can run on any machine) 

12- Now we need to build this image so now run the following command in the terminal -> docker build [ name your app ] [app location ]
example-> docker build -t react-app:dev . 

13- Make sure your docker instance is running in the background

14- docker run -p your port:port (imageid)
example -> docker run -p 5173:5173 aiugh87622323e

15- [specify what image we need to run]
so first check for the images by running the following command -> 
docker images ->Then select for the image you want and specify the image -> example -> docker run -p 5173:5173 aiugh87622323e

16- now check on the port that is your app running 

17- Now that we have our docker image built over here we can deploy it to cloud , share it to hub or anyone who wants to run this image on his system 
------------------------------------------------------------------
18- Now as we are ready with our image we will be using -> a Tool named [Acron] to push this image to cloud 

19- Go to acron.io -> login to your account there -> Go to docs -> 
and check for the installation command -> 
scoop install acron

20- so firstly go to scoop.sh and copy the installation lines and run them in your power shell to install scoop which is just a command line tool for installing some dependencies 

21- now restart your code editor for changes to take affect 
(and check if acron is installed successfully)

22- Now in src folder create a acron file -> Acronfile-> 
And here in the acron file we need to write some instructions here    

->
containers: app:{
build:{
context:"." (context will be current directory hence -> ".")
}
ports: publish: "your port so -> 5173/http"
if args.dev {
dirs:{
"/app":"./"
}
}
}



23-> now in the terminal run command -> acorn login -> and after a successful login 

24-> now all we have to do to run our app is in the terminal run the following command -> acorn run -i -n app . (app location so -> .)

25-> Also make sure to delete your node modules from the app it may or may not cause some problems while running the above command 

26-> Now open the address and check that your app has been deployed to the cloud and from there we can share our link to anyone and they can access the app

so till here its deployed in the dev mode so if we shut our server in our ide it will also shut down the deployed image 

27-> So instead we can also do delete that image and then in the terminal -> acorn run -n app .
[this will enable our app to run without the server in the ide]

Dockerfile insides -->
------------------------------
FROM node:20-alpine

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . . 

EXPOSE 5173
------------------------------


Docker wraps your app in form of an image and we can simply share this image with others .It can basically be said as a box that contains everything to run your app 

DOCKER IMAGE IS BASICALLY AN ISOLATED ENVIRONMENT , ITS LIKE AN EMPTY FOLDER -> YOU CAN INSTALL THINGS INSIDE OF IT , YOU CAN CREATE A FOLDER 
