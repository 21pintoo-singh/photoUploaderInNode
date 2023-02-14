File Upload in MongoDB using Node js:-

Upload image file to mongodb using node js; In this tutorial, you will learn how to upload image file in MongoDB database using Node js + Express + mongoose. And as well as learn how to store file/image in node js + express application directory and MongoDB database + mongoose.
Upload Image File to MongoDB using Node js + Express:-

Follow the following steps to upload and store image file in MongoDB using node js:
Step 1 – Create Node Express js App
Step 2 – Install express body-parser mongoose Multer dependencies
Step 3 – Connect App to MongoDB
Step 4 – Create Model
Step 5 – Create File/Image Upload HTML Markup Form
Step 6 – Import Modules in App.js
Step 7 – Start App Server

Step 1 – Create Node Express js App
Execute the following command on terminal to create node js app:
mkdir my-app
cd my-app
npm init -y

Step 2 – Install express ejs body-parser mongoose Multer Modules
Execute the following command on the terminal to express ejs body-parser mongoose dependencies :

npm install -g express-generator
npx express --view=ejs

npm install

npm install body-parser --save
npm install express multer --save
npm install mongoose
body-parser – Node.js request body parsing middleware which parses the incoming request body before your handlers, and make it available under req.body property. In other words, it simplifies the incoming request.

Express-EJS– EJS is a simple templating language which is used to generate HTML markup with plain JavaScript. It also helps to embed JavaScript to HTML pages

Mongoose – Mongoose is a MongoDB object modeling tool designed to work in an asynchronous environment. Mongoose supports both promises and callbacks.

Multer – Multer is a node.js middleware for handling multipart/form-data , which is primarily used for uploading files. It is written on top of busboy for maximum efficiency.

Step 3 – Connect App to MongoDB
Create database.js file into your app root directory and add the following code into it to connect your app to the mongodb database:

var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/node-file-upl', {useNewUrlParser: true});
var conn = mongoose.connection;
conn.on('connected', function() {
    console.log('database is connected successfully');
});
conn.on('disconnected',function(){
    console.log('database is disconnected successfully');
})
conn.on('error', console.error.bind(console, 'connection error:'));
module.exports = conn;

Step 4 – Create Model
Create Models directory and inside this directory create imageModel.js file; Then add following code into it:

const mongoose = require("../database");
 
var mongoose = require('mongoose');
 
var imageSchema = new mongoose.Schema({
    name: String,
    desc: String,
    img:
    {
        data: Buffer,
        contentType: String
    }
});
 
//Image is a model which has a schema imageSchema
 
module.exports = new mongoose.model('Image', imageSchema);

Step 5 – Create File/Image Upload HTML Markup Form
Create file/image upload Html form for upload image or file in mongoDB database; So visit views directory and create index.ejs file inside it. Then add the following code into it:


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>node js upload image to mongodb</title>
</head>
<body>
    <h1>Upload Image</h1>
    <form action="/uploadphoto" enctype="multipart/form-data" method="POST">
        <input type="file" name="myImage" accept="image/*">
        <input type="submit" value="Upload Photo">
    </form>
</body>
</html>

Step 6 – Import Modules in App.js
Import express, body-parser, mongoose, multer dependencies in app.js; as shown below:

const express = require("express");
const app = express();
const bodyParser = require("body-parser");
const path = require('path');
const fs = require("fs");
const multer = require("multer");
const mongoose = require("mongoose");
var imageModel = require('../models/imageModel');
 
 
app.use(bodyParser.urlencoded(
      { extended:true }
))
 
app.set("view engine","ejs");
 
// SET STORAGE
var storage = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, 'uploads')
    },
    filename: function (req, file, cb) {
      cb(null, file.fieldname + '-' + Date.now())
    }
  })
 
var upload = multer({ storage: storage })
 
app.get("/",(req,res)=>{
    res.render("index");

    })
 
app.post("/uploadphoto",upload.single('myImage'),(req,res)=>{
    var img = fs.readFileSync(req.file.path);
    var encode_img = img.toString('base64');
    var final_img = {
        contentType:req.file.mimetype,
        image:new Buffer(encode_img,'base64')
    };
    imageModel.create(final_img,function(err,result){
        if(err){
            console.log(err);
        }else{
            console.log(result.img.Buffer);
            console.log("Saved To database");
            res.contentType(final_img.contentType);
            res.send(final_img.image);
        }
    })
})
//Code to start server
app.listen(3000,function () {
      console.log("Server Started at PORT 2000");
})


Step 7 – Start App Server
You can use the following command to start node js app server:

//run the below command

npm start

after run this command open your browser and hit 

http://127.0.0.1:3000/

Conclusion
Node js + MongoDB + express file upload; In this tutorial, you have learned how to upload image files in the MongoDB database using Node js + Express.