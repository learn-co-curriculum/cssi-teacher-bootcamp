## Intro to App Engine

### GAE as a Connector
* Platform as a Service (Paas) to build/run applications hosted on Google's servers
* App Engine connects the back-end (the Python scripts) and front-end (the HTML/CSS/JavaScript) for you with minimal work on your end


### Creating a New App
* In Launcher, Create New app
* App Name should be only lower case, no underscores! (they will have no name it pig-latin-name)
* Pick Directory
* Look at files together
* Launch App
* Explain Ports

### Responding to Requests using Handlers and Routes
A user can make a request (ask students about the two different kinds) and we need to "handle" them, based on which type of request it was and which resource it was trying to access.

* Routes read the urls and decide which handler to execute
* Within the handler, you can define a get(self) or a get(post) method.

```
class PointlessHandler(webapp2.RequestHandler):
    def get(self):
        self.response.write("<html><form action='' method='get'><button name='get-pressed' value='true'>Make Get Request</button></form><form action='' method='post'><button name='post-pressed' value='true'>Make Post Request</button></form></html>")
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

####STUDENT PRACTICE: Routes & Handlers
Students will work in pairs to add a pigLatin handler, just like the count handler, they can add the method they wrote from Day 1.


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
1. Add jinja 2 to .yaml file and main.py

    Controller needs to go get that page and combine it with any data - a templating engine is needed to do all this cleanly.
    Jinja2 is one of the more popular and most popular templating engines for Python
    ```
     libraries:
    - name: jinja2
      version: latest
    ```

2. Import jinj2 and os in main.py

    ```python  
    import jinja2
    import os
    import webapp2
```
3. Set up Jinja Environment
   
     Jinja 2 needs to know where your HTML file is located. Set up Jinja directory to match current direction of the main.py file
    ```python
    jinja_environment = jinja2.Environment(
      loader=jinja2.FileSystemLoader(os.path.dirname(__file__)))#this little bit sets jinja's relative directory to match the directory name(dirname) of the current __file__, in this case, helloworld.py
    ```
4. Use get_template method on your jinja_environment path
   
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

####STUDENT PRACTICE: Rendering Templates
Students will work in pairs to make Pig Latin template and render it.


