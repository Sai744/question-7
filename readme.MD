In this question I have taken 2 sample docker files(mysql, backend) .I have taken sample nodejs application . Build those docker files checked the logs and push those into ECR(Elastic container registry).

Initially checked images and containers. 

--

This is mysql docker file. 

--

Explanation of the dockerfile:

FROM command is used for importing the mysql image from dockerhub. 
ENV command is used for Setting the environment variables . that means setting the password for the db.
RUN command used to add expense user in group and give ownership for that user to that path 
ADD command copies the files(scripts) from local to image.
USER command used to create the user. This should be the last command or last but one command in the docker file to avoid getting root access to the containers.

---

Building the mysql image and running as container.

--

Now we can see that the mysql image and its container are running

--

Checking the logs of the container

--

This is backend dockerfile:

--

Explanation of the dockerfile:


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

--

--

Here iam pushing the created backend , mysql images in ECR using terraform

--

Pushed images are seen in the ECR. 

--

Follow these push commands to push the image .

--

Now the image has been pushed to ECR.

--