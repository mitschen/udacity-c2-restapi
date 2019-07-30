
# Udagram Image Filtering Microservice

  

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

  

The project is split into three parts:

1.  [The Simple Frontend](https://github.com/grutt/udacity-c2-frontend)

A basic Ionic client web application which consumes the RestAPI Backend.

2.  [The RestAPI Backend](https://github.com/grutt/udacity-c2-restapi), a Node-Express server which can be deployed to a cloud service.

3.  [The Image Filtering Microservice](https://github.com/grutt/udacity-c2-image-filter), the final project for the course. It is a Node-Express application which runs a simple Python script to process images.

  

## Tasks

### Setup Python Environment

You'll need to set up and use a virtual environment for this project.

  

To create a virtual environment run the following from within the project directory:

1. Install virtualenv dependency: `pip install virtualenv`

2. Create a virtual environment: `virtualenv venv`

3. Activate the virtual environment: `source venv/bin/activate` (Note: You'll need to do this every time you open a new terminal)

4. Install dependencies: `pip install -r requirements.txt`

  

When you're done working and leave the virtual environment, run: `deactivate`

  

### Setup Node Environment

You'll need to create a new node server. Open a new terminal within the project directory and run:

1. Initialize a new project: `npm init`

2. Install express: `npm i express --save`

3. Install typescript dependencies: `npm i ts-node-dev tslint typescript @types/bluebird @types/express @types/node --save-dev`

4. Look at the `package.json` file from the RestAPI repo and copy the `scripts` block into the auto-generated `package.json` in this project. This will allow you to use shorthand commands like `npm run dev`

  

### Create a new server.ts file

Use our basic server as an example to set up this file. For this project, it's ok to keep all of your business logic in the one server.ts file, but you can try to use feature directories and app.use routing if you're up for it. Use the RestAPI structure to guide you.

  

### Add an endpoint to handle POST /imagetoprocess requests

It should accept two POST parameter:

> image_url: string - a public URL of a valid image file

  

> upload_image_signedUrl: string (OPTIONAL) - a URL which will allow a PUT request with the processed image

It should respond with 422 unprocessable if either POST parameters are invalid.

  

It should require a token in the Auth Header or respond with 401 unauthorized.

  

It should be versioned.

  

> The matching token should be saved as an environment variable

> (TIP we broke this out into its own auth.router before, but you can access headers as part of the req.headers within your endpoint block)

  

It should respond with the image as the body if upload_image_signedUrl is included in the request.

  

It should respond with a success message if upload_image_signedUrl is NOT included in the request.

  
  

### Refactor your RestApi server

#### Add a request to the image-filter server within the RestAPI POST feed endpoint

  

It should create new SignedURLs required for the imagetoprocess POST Request body.

  

It should include a POST request to the new server (TIP keep the server address and token as environment variables).

  

It should overwrite the image in the bucket with the filtered image (in other words, it will have the same filename in S3).

  
  

### Deploying your system!

Follow the process described in the course to `eb init` a new application and `eb create` a new environment to deploy your image-filter service!

  
  

## Stand Out

#### Postman Integration Tests

Try writing a postman collection to test your endpoint. Be sure to cover:

> POST requests with and without tokens

> POST requests with valid and invalid parameters

  

#### Refactor Data Models

Try adding another column to your tables to save a separate key for your filtered image. Remember, you'll have to rename the file before adding it to S3!

  

#### (ADVANCED) Refactor Data Models

Try adding a second OpenCV filter script and add an additional parameter to select which filter to use as a POST parameter


----

# Own notes
* I was facing many troubles installing the stuff on Ubuntu 18.04. The major issues came up, when i've tried to deploy the udacity-c2-restapi on the AWS backend - which never worked on my side using the `eb console`.
	* nevertheless, after many discussions with my tutor (thanks to Menaf here) I was finally able to deploy the rest-api on the AWS backend. Unfortunately this screwed up the possibility to run the **udacity-c2-frontend** on my machine :-(... long story short, everything works perfect out of the box.
* so what i've done
	* I've installed node 12 according to the following [howto](https://github.com/nodesource/distributions/blob/master/README.md). Previously I've runned everything with node 10 (which was working fine locally, but never works on the AWS - which at this time supports 10.16 only). This will allow you to run the stuff locally but I've failed to run the `npm run build` command due to **sequelize**
	* so to solve the `npm run build`issues with sequelize, install the latest version of sequelize according to the [install notes](https://www.npmjs.com/package/sequelize-typescript). 
	* to run the solution locally, type `npm run start`which will simply call `node . `which is executed on the AWS as well. If you're running into a error, that main application not found, change the `"main": "src/server.js"`to `"main": "www/server.js"`in the package.json
* this will now work on my local machine, but didn't work on the AWS using the `eb console`commands. So what to do - deploy is manually using the AWS console
	* enter the AWS console via webbrowser and go to elastic beanstalk
	* Choose "create new application", select "WebApplication" with Node.js as environment and select the Archive.zip which is the result of the `npm run build`step. This at least works on my side.
