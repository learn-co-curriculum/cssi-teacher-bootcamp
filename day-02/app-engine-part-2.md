
### Using Variables in Templates
In Jinja  the syntax for interpolating strings is to put the expression between mustaches, like this: {{ variable }}. 

In hello.html, instead of Hello World, write Hello {{ name }}. Now our page will change to greet anyone whose name is stored in the variable 'name'.

### Passing Variables to the Template
The .render() method takes an optional variable where you can pass a dictionary of key:value pairs
The key is always in quotes, and should be the same as the name in the template
```python
class HelloHandler(webapp2.RequestHandler):
    template = jinja_environment.get_template('templates/hello.html')
    self.response.out.write(template.render( {"name": "CSSI Chicago"}))
```

####STUDENT PRACTICE: Adding template Variables
Students work in pairs to pass in a new variable, called greeting. First prompt them about where the variable needs to go (inside the render parameter)


## Getting Data from the Request
Often the data we want to use is coming from the user.

### The GET Method
* recieves data based on URL
* Everything after the ? in a url is called the query string. 

`https://www.facebook.com/login.php?login_attempt=1`


#### The Query String
Query strings are almost always a set of paired parameter names and values, separated by ampersands (&). A complex one might look like this:

`http://api.umd.io/v0/courses?dept_id=CMSC&semester=201501&credits=3&department=Computer%20Science&grading_method=Audit`

The route is api.umd.io/v0/courses and the url parameters are:

| dept_id       | CMSC        | 
| ------------- |-------------|
| semester      | 20150       | 
| credits       | 3           | 
| department     | Computer Science   | 
|grading_method|Audit|



### Accessing the URL Parameters
* Use self.request.get() to access the url parameters
* Ask students how to modify url to add "name" to the query string

Here's an updated HelloWorld Handler in helloworld.py that gets the 'name' parameter from the url and passes it into the template.
```python
class HelloHandler(webapp2.RequestHandler):
  def get(self):
    template_vars = {'name': self.request.get('name')}
    template = jinja_environment.get_template('templates/hello.html')
    self.response.out.write(template.render(template_vars))
```

####STUDENT PRACTICE: Accessing URL Parameters
Students add greeting to the query string
 
### Data from Forms (Post Method)
#### HTML Forms (Review)

The form element has two attributes: _method_ and _action_.
* The _method_ attribute specifies the HTTP method (GET or POST) to be used when submitting the forms. 
* The _action_ attribute tells which script to run or which page to return to once the submit button is pressed. Can leave blank since handler will take care of routing/actions

```
	<form method="post" action ="">
		<p>What is your first name? <input type="text" name="firstName"/></p>
		<p>What is your last name? <input type="text" name="lastName"/></p>
		<p><input type="submit" value="Greet Me!"></p>
	</form>
```


####Adding the  Post Method
* add a post method to your Handler

```python
class MainHandler(webapp2.RequestHandler):
    def get(self):
    	main_template = jinja_environment.get_template('templates/main.html')
    	self.response.out.write(main_template.render())
    def post(self): 
    	self.response.out.write("You have submitted your quiz")
```

####Rendering a Template using the POST data
* still use self.request.get() with the parameters being the name of the form fields
* purposely call the keys/values different in the lesson so students can see the key is being passed to the template
```python
class MainHandler(webapp2.RequestHandler):
    def get(self):
    	main_template = jinja_environment.get_template('templates/main.html')
    	self.response.out.write(main_template.render())
    def post(self): 
    	results_template=jinja_environment.get_template('templates/results.html')
    	my_vars={"user_fname":self.request.get("firstName"), "user_lname":self.request.get("lastName")}
    	self.response.out.write(results_template.render(my_vars))
```
####STUDENT PRACTICE: Using Form Data on POST Method
Students add a new field, City to the form. This gets displayed in the results template
