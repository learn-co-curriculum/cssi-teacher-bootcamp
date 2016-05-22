## Intro to MVC
MVC is the basic framwork that describes all apps, how a user is able to interact with a machine.

### The 3 Parts of MVC
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

### STUDENT MVC PRACTICE
Break into teams of 2/3 to brainstorm their own anology. What is a common interface/intermediary between a user and what it wants?
* Ice Skater - Clerk - Shoe Sizes
  *Get Request - ask how much money it is to rent skates, asks how many half sizes are left
  *Post Request - asks to rent a size 8.5 
* Response - Skates come out of storage, given to skater from clerk


## HTTP, Clients, Servers, Requests

### STUDENT HTTP PRACTICE
As a class, students should be drawing a diagram in pairs. 
Have students pass the marker between partners while they listen to the explanation.

### Clients and Servers
* Internet = network of connected computers - communicate with HTTP - defines how data is delivered
* Computers can be clients, servers or both
![Client and Server Diagram](https://mdn.mozillademos.org/files/4291/client-server.png)

### Requests & Responses
Your webapp needs to be able to get data (a request) from a user and send data back from the server (a response).

There are two ways that a client (users) can make a request (send data): **GET** and **POST**
 + GET - requests/retrieves data - viewing something - data stored as URL parameters
 + POST - submits data -changing something (update or adding) - data is hidden (sent in HTTP body)

 
