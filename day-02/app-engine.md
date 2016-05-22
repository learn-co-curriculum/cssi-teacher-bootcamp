## Intro to App Engine

### GAE as a Connector
* Platform as a Service (Paas) to build/run applications hosted on Google's servers
* App Engine connects the back-end (the Python scripts) and front-end (the HTML/CSS/JavaScript) for you with minimal work on your end


### Creating a New App
* In Launcher, Create New app
* App Name should be only lower case, no underscores!
* Pick Directory
* Look at files together
* Launch App
* Explain Ports

### Responding to Requests using Handlers and Routes
A user can make a request (ask about the two different kinds) and we need to "handle" them, based on which type of request it was and which resource it was trying to access.

Routes handles the urls and decide which Handler to Execute
Within the handler, you can define a get(self) or a get(post) method.

```HTML
<form action="" method="get">
      <button name="button-pressed" value="get">Submit Get Request</button>
    </form>
    <form action="" method="post">
      <button name="button-pressed" value="post">Submit Post Request</button>
    </form>
```

```
class PointlessHandler(webapp2.RequestHandler):
    def get(self):
        self.response.write('You pressed the GET button!') 
    def post(self):
        self.response.write('You pressed the POST button!') 

```

```python
class MainHandler(webapp2.RequestHandler):
    def get(self):
        self.response.write('Hello world!')

class CountHandler(webapp2.RequestHandler):
    def get(self):
        for i in range(1, 11):
            self.response.write(i)

app = webapp2.WSGIApplication([
    ('/', MainHandler),
    ('/count', CountHandler),
], debug=True)
```

MainHandler and CountHandler both respond in different ways. Ask students:
* What MainHandler will do? (displays "Hello World") 
* What will CountHandler do? (print 1-10). 
* What url will run the CountHandler? (/count)


As a class, students will work in pairs to add at least one other  handler/route.


### Templates
Right now our controller can respond with text only. We can add elements and styling to our pages by using templates. 
Templates true power comes because each HTML page can dynamically change.

####Make HTML Page
Your HTML page should be created in a new Templates folder
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Howdy</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    <img height="100px" width="150px" src="https://blackgeoscientists.files.wordpress.com/2014/06/helloworld.jpg" alt="A cute Pic of a Dude on the World">
    <h2>How are ya?</h2>
  </body>
</html>
```


#### Add Jinja2
Add jinja 2 to .yaml file and main.py
Controller needs to go get that page and combine it with any data - a templating engine is needed to do all this cleanly.
Jinja2 is one of the more popular and most popular templating engines for Python
```
 libraries:
- name: jinja2
  version: latest
```

Edit the handler to grab your new HTML file:

```python  
import jinja2
import os
import webapp2
```

Jinja 2 needs to know where your HTML file is located. Set up Jinja directory to match current direction of the main.py file
```python
jinja_environment = jinja2.Environment(
  loader=jinja2.FileSystemLoader(os.path.dirname(__file__)))#this little bit sets jinja's relative directory to match the directory name(dirname) of the current __file__, in this case, helloworld.py
```

Get the template you want by using the get_template method, and passing the path to the HTML file. Then render that template. Render is the magic verb that combines the HTML with any embedded logical code or data
```python  
import jinja2
import os
import webapp2

jinja_environment = jinja2.Environment(
  loader=jinja2.FileSystemLoader(os.path.dirname(__file__)))#this little bit sets jinja's relative directory to match the directory name(dirname) of the current __file__, in this case, helloworld.py

class MainHandler(webapp2.RequestHandler):
    def get(self):
        hello_template = jinja_environment.get_template('templates/hello.html')
        self.response.write(hello_template.render())
```


Students will change their count handler to display a template with the numbers 1-10.

#### Adding jinja2 Logic
Instead of writing 1-10 by hand, we can infuse logic directly into our template. 
To do this, you need to use curly-brackets with parenthesis {% for number in range(1:11) %} to let templating engine know it should be interpreted as code.
Each variable is in double mustaches {{number}}
```
<!DOCTYPE  html>
<html>
  <head>
    <title>Count!</title>
  </head>
  <body>
    {% for number in range(1,11) %}
    <p>{{number}}</p>
    {% endfor %}
  </body>
</html>
```
With separation of concerns, logic in the HTML should be limited to add/expand/duplicate HTML tag structure - anything else should be done in the controller. 
