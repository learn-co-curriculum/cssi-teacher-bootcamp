## Intro to App Engine
### MVC
<img src="http://lh3.ggpht.com/aviadezra/SHj6gLRSkSI/AAAAAAAAALg/0xkCGOXuefc/image_thumb3.png?imgmax=800" width=300px>
+ **Models** (Back-end data - Datastore in App Engine)
  + Stores the data
+ **Views** (Front-end - HTML Template in App Engine)
  + The front end, this is what the user sees.
+ **Controller** (Back-end logic - our Python scripts in App Engine)
 + The code that is in charge of making the back end communicate to the front end

### Restaurant Analogy
* Kitchen - Models - stores food (data), prepares the right food (data) to send back
* Customer - Views -  asks questions about specials (Get Request), submits order (Post request)
* Waiter - Controller - ferries data back and forth
* Requests/Responses
  * Get Request - Customer asks about specials (Waiter needs to response to patron - here are pictures, or specials)
  * Post Request - Customer places order (submitting this form all the way to the kitchen)
  * Response - The waiter answers a question, the food comes back from the kitchen (200) or they say "we can't find that meal (404)", or they give you something else (redirect 303?)

Break into teams of 2/3 to brainstorm their own anology. What is a common interface/intermediary between a user and what it wants?
* Ice Skater - Clerk - Shoe Sizes
  *Get Request - ask how much money it is to rent skates, asks how many half sizes are left
  *Post Request - asks to rent a size 8.5 
* Response - Skates come out of storage, given to skater from clerk

### GAE as a Connector
* Platform as a Service (Paas) to build/run applications hosted on Google's servers
* App Engine connects the back-end (the Python scripts) and front-end (the HTML/CSS/JavaScript) for you with minimal work on your end.

As a class, students should be drawing this diagram in pairs. Have students pass the marker between partners while they listen to the explanation.
### Clients and Servers
* Internet = network of connected computers - communicate with HTTP - defines how data is delivered
* Computers can be clients, servers or both
![Client and Server Diagram](https://mdn.mozillademos.org/files/4291/client-server.png)

### Requests
Your webapp needs to be able to get data (a request) from a user and send data back from the server (a response).

There are two ways that a client (users) can make a request (send data): **GET** and **POST**
 + GET - requests/retrieves data - viewing something - data stored as URL parameters
 + POST - submits data -changing something (update or adding) - data is hidden (sent in HTTP body)
 
 

### Responding to Requests using Handlers and Routes
User enters url or presses button
Your controller needs to respond (based on it it was a get or a post)

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

MainHandler and CountHandler both respond in different ways. 
Ask what MainHandler will do? (displays "Hello World") 
What will CountHandler do? (print 1-10). 
What url will run the CountHandler? (/count)


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
        self.response.out.write(hello_template.render())
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
