# EVL OrientDB CRUD Generator

Epsilon EVL demo project to generate OrientDB CRUD implementation using the Graph API

##  DISCLAIMER

I am **NOT** an experienced DB programer and thus the generated code will most likely be "ugly"; incorrect use of API, incorrect separation of concerns, incorrect use of Frames and Pipes, etc. The purpose is to ilustrate how EVL can be used to generate CRUD code for a DB, not how to write CRUD code ;). Ergo, this code is not fully functional. Just a skeleton with basic functionality.

## Requirements

You need an Eclipse IDE with EMF and Epsilon installed as minimum. The easiest way is to get one from the Epsilon website, [here](https://eclipse.org/epsilon/download/).

You need then to import the plugins in this repo to your Eclipse workspace:
  
    File -> Import -> Existing Projects into Workspace (follow the instructions)
    
**Note:** I have not commited any of the generated code so after importing your projects will most likely won't compile and you will get a lot of 'The import xxx cannot be resolved'.


## Runing the generators

We need to generate two pieces of code: the Data Model (defined by the business layer) and the DB code (used in the data layer). The Data Model code is generated using EMF's generator model. 

1. Open the ExecutionTrace.genmodel (org.eclipse.epsilon.orientdb.business/model). Right click on the firt element in the tree view and select: Generate Model Code. A bunch of files should have been generated in the emf-gen folder. Note that the EMF genmodel uses it's own internal generators. 

2. Locate the EcoreToOrientDB_Demo.launch at the root of the org.eclipse.epsilon.orientdb.data project. Right click on it and select Run as.. > EcoreToOrientDB_Demo. You should see a *GenDone* message in the console and a bunch of pacakges and files should have been generated in the src-gen folder.

## The generated code

1. The EMF generator model generates the Data Model API. Basically, we are interested in the Interfaces generated in org.eclipse.epsilon.orientdb.business/emf-gen/org/eclipse/epsilon/orientdb/business/trace/. POJOs implementing this interfaces are intended to be used to move data between the laters. For example, the generated OrientDB code provides classes that implement these interfaces.

2. The EcoreToOrientDB_Demo launch executes the org.eclipse.epsilon.orientdb.data/src/org/eclipse/epsilon/orientdb/data/generation/EcoreToOrientDB.egx EGX Script. This script is responsible for orchestrating the different EVL scripts that generate the DB code. The DB code includes Vertex and Edge definitions to provide a DB schema (sr-gen/org.eclipse.epsilon.orientdb.data.trace), implementations of the Data Model that delegate to DB objects (srg-gen/org.eclipse.epsilon.orientdb.data.trace.impl/) and a DAO class that provides the CRUD methods for each of the Data Model classes (src-gen/org.eclipse.epsilon.orientdb.data.execute/TraceOrientDbDAO.java)

Both projects should compile and be free of errors. However, this code is not production ready!!! If you want to see how the generated code works, you can instantiate the /org.eclipse.epsilon.orientdb.data/src/org/eclipse/epsilon/orientdb/data/OrientDbTraceManager.java class and use its API to create new traces on the DB and query for existing traces.

**Note:** The provided code only has support for connecting to In-Memory databases.

The next sections explain how the generators work so you can modify them for your own Data Model, improve the CRUD code and provide better sinergy with your project. 

You would proably want to understand the Data Model modelling first and then dive into understanding the templates. 

**Note:** There are a few places (mainly the import statements) in which the templates are harcoded to point to the packages used in this example (e.g. the Data Model interfaces are expected to be in the org.eclipse.epsilon.orientdb.business.trace pacakge. You will need to adjust this, or improve the templates so that the import statements are also created dynamically. :)

## Customizaing the Generators

[The Data Model](data_model.md)
[The EVL Templates](data_model.md)

