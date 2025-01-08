In this question I have taken 2 sample docker files(mysql, backend) .I have taken sample nodejs application . Build those docker files checked the logs and push those into ECR(Elastic container registry).

Initially checked images and containers. 

![20](https://github.com/user-attachments/assets/4ff555a1-9a27-4158-a850-a8c8604efc34)

* Explanation of the mysql dockerfile:

FROM command is used for importing the mysql image from dockerhub. 
ENV command is used for Setting the environment variables . that means setting the password for the db.
RUN command used to add expense user in group and give ownership for that user to that path 
ADD command copies the files(scripts) from local to image.
USER command used to create the user. This should be the last command or last but one command in the docker file to avoid getting root access to the containers.

![21](https://github.com/user-attachments/assets/94e68626-e732-4f48-bd43-5c83c8c89ad1)

Building the mysql image and running as container.

![22](https://github.com/user-attachments/assets/1c560b41-fb8d-49d8-a8ac-e45904ca9b40)

Now we can see that the mysql image and its container are running

![23](https://github.com/user-attachments/assets/b50d823e-6d0a-41d4-8c32-a30eb48ea306)

Checking the logs of the container

![24](https://github.com/user-attachments/assets/a0782d94-68f0-4a6f-b5ba-3928d17dae62)


* Explanation of the dockerfile:


From command imports official Node.js 20 image as the base image for the builder stage .
Here iam using multistage build as it helps to reduce the final image size.
WORKDIR sets the working directory to /opt/server.
COPY command copies all the content from the locak machine to the working directory.
RUN npm intsall installs the Node.js dependencies listed in the package.json file.
FROM node:20.18.0-alpine3.20 starts a new stage using a different base image (node:20.18.0-alpine3.20), which is an Alpine-based Node.js image. Alpine images are minimal and smaller in size.
EXPOSE exposes port 8080 from the container, meaning your application will listen on port 8080 when running.
ENV This sets an environment variable DB_HOST to "localhost".
RUN creates a new user group (expense) and user (expense) in the container.
Mkdir creates the /opt/server directory and changes the ownership of the directory and its contents (recursively) to the expense user and group.
CMD  sets the default command to run when the container starts. In this case, it runs the index.js file with Node.js.

![25](https://github.com/user-attachments/assets/34311e20-29eb-4f8d-ab20-ee4fe8a80dd2)

![26](https://github.com/user-attachments/assets/17751ead-8b8d-4575-8b9f-d996b5900631)

Here iam pushing the created backend , mysql images in ECR using terraform

![27](https://github.com/user-attachments/assets/810e07f1-1404-43db-94c4-d19e7e73c677)

Pushed images are seen in the ECR. 

![28](https://github.com/user-attachments/assets/7e9813b8-317c-4cff-b525-640870e2fe48)

Follow these push commands to push the image .

![29](https://github.com/user-attachments/assets/9abc2fac-a6da-4c8a-973d-1c95cdd6171f)

Now the image has been pushed to ECR.

![30](https://github.com/user-attachments/assets/d5f950c5-fd48-41c2-b051-0f78c03f5c7a)