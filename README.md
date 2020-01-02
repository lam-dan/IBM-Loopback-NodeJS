# todo-list
Created a simple todo-list application using IBM Loopback 4 framework which uses Node.js and based on Express that enables you to quickly create dynamic end-to-end REST APIs and connect to backend systems such as databases and SOAP or REST services.

[![LoopBack](https://github.com/strongloop/loopback-next/raw/master/docs/site/imgs/branding/Powered-by-LoopBack-Badge-(blue)-@2x.png)](http://loopback.io/)




# Models

## TodoList
* unique id
* title
* color to represent the TodoList with

# Datasources

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

## Create your repository

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

## Create your controller

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


