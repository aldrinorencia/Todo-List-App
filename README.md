# Todo List Application

## Setup
- Create an empty directory named `todo-app`
- Open up the console, navigate to the directory and run `npm init`
- Fill out the required information to initialize the project
- Within the todo-app directory, create a new file named `index.js` this is where we will put most of our code.
## Server Setup
To get our server up and running we need to use **Express**. Express is a minimalist web framework for Node.js, it makes it very easy to create and run a web server with Node. 
Install express in the console by running:
<br>
<br>`npm install express --save`
<br>
<br>
Once installed we can then add the following code into the `index.js` file
<pre>
//dependencies required for the app
var express = require("express");
var bodyParser = require("body-parser");
var app = express();

// Body Parser middleware
app.use(bodyParser.urlencoded({extended: true}));
// setup the template engine
app.set('view engine', 'ejs');
//render css files
app.use(express.static('public'));

//placeholders for added task
var task = ["buy shoes", "nodejs sample"];
//placeholders for removed task
var complete = ["finish jquery"];

//post route for adding new task 
app.post('/addtask', function(req, res) {
    var newTask = req.body.newtask;
    //add the new task from the post route
    task.push(newTask);
    res.redirect("/");
});

app.post('/removetask', function(req, res) {
    var completeTask = req.body.check;
    //check for the "typeof" the different completed task, then add into the complete task
    if (typeof completeTask === 'string') {
        complete.push(completeTask);
        //check if the completed task already exits in the task when checked, then remove it
        task.splice(task.indexOf(completeTask), 1);
    } else if (typeof completeTask === 'object') {
        for (var i = 0; i < completeTask.length; i++) {
            complete.push(completeTask[i]);
            task.splice(task.indexOf(completeTask[i]), 1);
        }
    }
    res.redirect("/");
});

//render the ejs and display added task, completed task
app.get('/', function(req, res) {
    res.render('index', { task: task, complete: complete });
});

//set app to listen on port 3000
app.listen(3000, function() {
    console.log('server is running on port 3000');
});
</pre>

## View Setup
When anyone visits our root route, we would send back an HTML file that will have different heading text, buttons and a form for adding new task. And for that, we’ll be using **EJS** (Embedded JavaScript). EJS is a templating language.
Install EJS by running:
<br>
<br>`npm install ejs --save`
<br>
<br>
EJS is accessed by default in the `views` directory. So create a new folder named `views` in your directory. Within that `views` folder, add a file named `index.ejs`. Think of our `index.ejs` file as an HTML file.
<br>
<br> Add the following code into the `index.ejs` file
<br>
<br>`<html>`
<br>`<head> `
<br>`<title> ToDo App </title>`
 <br>`<link href="https://fonts.googleapis.com/css?family=Lato:100" rel="stylesheet">`
 <br>`<link href="/styles.css" rel="stylesheet">`
<br>`</head>`
<br>`<body>`
<br>`<div class="container">`
<br>`<h2> ToDo List App </h2>`
<br>`<form action ="/addtask" method="POST">`
<br>`<input type="text" name="newtask" placeholder="Add new task">`
<br>`<button> ADD TASK </button>`
<br>`<h2> Added Task </h2>`
<br>`<% for( var i = 0; i < task.length; i++){ %>`
    <br>`<li><input type="checkbox" name="check" value="<%= task[i] %>" /> <%= task[i] %> </li>`
<br>`<% } %>`
<br>`<button formaction="/removetask" type="submit" id="top"> REMOVE </button>`
<br>`</form>`
<br>`<h2> Completed task </h2>`
 <br>`<% for(var i = 0; i < complete.length; i++){ %>`
    <br>`<li><input type="checkbox" checked><%= complete[i] %> </li>`
<br>`<% } %>`
<br>`</div>`
<br>`</body>`
<br>`</html>`
<br>
Create a new folder named `public` in your directory. Within that `public` folder, add a file named `styles.css`. this will be our css file.
<br>
<br>Add the following code into the `styles.css` file
<br>
<pre>
* {
    font-family: "lato", sans-serif;
    font-weight: bold;
}

body {
    background: rgb(245, 245, 245);
    color: #333333;
    margin-top: 20px;
}

.container {
    display: block;
    width: 400px;
    margin: 0 auto;
}

ul {
    margin: 0;
    padding: 0;
}
</pre>
