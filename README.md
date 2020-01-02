# todo-list
Created a simple todo-list application using IBM Loopback 4 framework which uses Node.js and based on Express that enables you to quickly create dynamic end-to-end REST APIs and connect to backend systems such as databases and SOAP or REST services.

[![LoopBack](https://github.com/strongloop/loopback-next/raw/master/docs/site/imgs/branding/Powered-by-LoopBack-Badge-(blue)-@2x.png)](http://loopback.io/)

# Models
Now we can begin working on the representation of our data for use with LoopBack 4. To that end, we’re going to create a Todo model that can represent instances of a task for our Todo list. The Todo model will serve both as a Data Transfer Object (also known as a DTO) for representing incoming Todo instances on requests, as well as our data structure for use with loopback-datasource-juggler.

A model describes business domain objects and defines a list of properties with name, type, and other constraints.

Models are used for data exchange on the wire or between different systems.

### Building your Todo model
A todo list is all about tracking tasks. For this to be useful, it will need to let you label tasks so that you can distinguish between them, add extra information to describe those tasks, and finally, provide a way of tracking whether or not they’re complete.

The to-do model has the following properties:
* id: a unique id
* title: a title
* desc: a description that details the specific task to be accomplished
* isComplete: a boolean flag for whether or not we’ve completed the task

We can use the lb4 model command and answer the prompts to generate the model for us. Press return with an empty property name to generate the model. Follow these steps:

```
lb4 model
? Model class name: todo
? Please select the model base class: Entity
? Allow additional (free-form) properties? No
Model Todo will be created in src/models/todo.model.ts

Let's add a property to Todo
Enter an empty property name when done

? Enter the property name: id
? Property type: number
? Is id the ID property? Yes
? Is it required?: No
? Is id generated automatically? No
? Default value [leave blank for none]:

Let's add another property to Todo
Enter an empty property name when done

? Enter the property name: title
? Property type: string
? Is it required?: Yes
? Default value [leave blank for none]:

Let's add another property to Todo
Enter an empty property name when done

? Enter the property name: desc
? Property type: string
? Is it required?: No
? Default value [leave blank for none]:

Let's add another property to Todo
Enter an empty property name when done

? Enter the property name: isComplete
? Property type: boolean
? Is it required?: No
? Default value [leave blank for none]:

Let's add another property to Todo
Enter an empty property name when done

? Enter the property name:

   create src/models/todo.model.ts
   update src/models/index.ts

Model Todo was created in src/models/
```

# Datasources
Datasources are LoopBack’s way of connecting to various sources of data, such as databases, APIs, message queues and more. A DataSource in LoopBack 4 is a named configuration for a Connector instance that represents data in an external system. The Connector is used by legacy-juggler-bridge to power LoopBack 4 Repositories for Data operations.

In LoopBack 4, datasources can be represented as strongly-typed objects and freely made available for injection throughout the application. Typically, in LoopBack 4, datasources are used in conjunction with Repositories to provide access to data.

### Building a Datasource
From inside the project folder, we’ll run the lb4 datasource command to create a DataSource. For the purposes of this tutorial, we’ll be using the memory connector provided with the Juggler.

```
lb4 datasource
? Datasource name: db
? Select the connector for db: In-memory db (supported by StrongLoop)
? window.localStorage key to use for persistence (browser only):
? Full path to file for persistence (server only): ./data/db.json

  create src/datasources/db.datasource.config.json
  create src/datasources/db.datasource.ts
  update src/datasources/index.ts

Datasource Db was created in src/datasources/
```

Created a json located file in ./data/db.json
```
{
  "ids": {
    "Todo": 7
  },
  "models": {
    "Todo": {
      "1": "{\"title\":\"Take over the galaxy\",\"desc\":\"MWAHAHAHAHAHAHAHAHAHAHAHAHAMWAHAHAHAHAHAHAHAHAHAHAHAHA\",\"id\":1}",
      "2": "{\"title\":\"destroy alderaan\",\"desc\":\"Make sure there are no survivors left!\",\"id\":2}",
      "3": "{\"title\":\"play space invaders\",\"desc\":\"Become the very best!\",\"id\":3}",
      "4": "{\"title\":\"crush rebel scum\",\"desc\":\"Every.Last.One.\",\"id\":4}",
      "5": "{\"title\":\"get the milk\",\"id\":5,\"desc\":\"need milk for cereal\"}",
      "6": "{\"title\":\"hello world\",\"id\":6}"
    }
  }
}
```

# Repositories
The repository pattern is one of the more fundamental differences between LoopBack 3 and 4. In LoopBack 3, you would use the model class definitions themselves to perform CRUD operations. In LoopBack 4, the layer responsible for this has been separated from the definition of the model itself, into the repository layer.

A Repository represents a specialized Service interface that provides strong-typed data access (for example, CRUD) operations of a domain model against the underlying database or service.

### Create your repository
From inside the project folder, run the lb4 repository command to create a repository for your to-do model using the db datasource from the previous step. The db datasource shows up by its class name DbDataSource from the list of available datasources.

```
lb4 repository
? Please select the datasource DbDatasource
? Select the model(s) you want to generate a repository Todo
   create src/repositories/todo.repository.ts
   update src/repositories/index.ts
? Please select the repository base class DefaultCrudRepository (Legacy juggler
bridge)

Repository TodoRepository was created in src/repositories/
```

The `src/repositories/index.ts` file makes exporting artifacts central and also easier to import.

The newly created `todo.repository.ts` class has the necessary connections that are needed to perform CRUD operations for our to-do model. It leverages the Todo model definition and ‘db’ datasource configuration and retrieves the datasource using Dependency Injection.

# Controllers
In LoopBack 4, controllers handle the request-response lifecycle for your API. Each function on a controller can be addressed individually to handle an incoming request (like a POST request to /todos), to perform business logic, and to return a response.

Controller is a class that implements operations defined by application’s API. It implements an application’s business logic and acts as a bridge between the HTTP/REST API and domain/database models.

In this respect, controllers are the regions in which most of your business logic will live!

### Create your controller
You can create a REST controller using the CLI as follows:

```
lb4 controller
? Controller class name: todo
Controller Todo will be created in src/controllers/todo.controller.ts

? What kind of controller would you like to generate? REST Controller with CRUD functions
? What is the name of the model to use with this CRUD repository? Todo
? What is the name of your CRUD repository? TodoRepository
? What is the name of ID property? id
? What is the type of your ID? number
? Is the id omitted when creating a new instance? Yes
? What is the base HTTP path name of the CRUD operations? /todos
   create src/controllers/todo.controller.ts
   update src/controllers/index.ts

Controller Todo was created in src/controllers/
```

Let’s review the TodoController located in `src/controllers/todo.controller.ts`. The @repository decorator will retrieve and inject an instance of the TodoRepository whenever an inbound request is being handled. The lifecycle of controller objects is per-request, which means that a new controller instance is created for each request. As a result, we want to inject our TodoRepository since the creation of these instances is more complex and expensive than making new controller instances.

# Putting it all together
We’ve got all of our artifacts now, and they are all automatically bound to our Application so that LoopBack’s Dependency injection system can tie it all together for us!

LoopBack’s boot module will automatically discover our controllers, repositories, datasources and other artifacts and inject them into our application for use.

NOTE: The boot module will discover and inject artifacts that follow our established conventions for artifact directories. Here are some examples:

Controllers: ./src/controllers
Datasources: ./src/datasources
Models: ./src/models
Repositories: ./src/repositories
To find out how to customize this behavior, see the Booters section of Booting an Application.

Let’s try out our application! First, you’ll want to start the app.
```
$ npm start

Server is running at http://127.0.0.1:3000
```
Next, you can use the API Explorer to browse your API and make requests at http://localhost:3000/explorer/

Here are some requests you can try:

* POST `/todos` with a body of `{ "title": "get the milk" }`
* GET `/todos/{id}` using the ID you received from your POST, and see if you get your Todo object back.
* PATCH `/todos/{id}`, using the same ID, with a body of `{ "desc": "need milk for cereal" }`
That’s it! You’ve just created your first LoopBack 4 application!

