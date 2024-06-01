
# Data models

It defines how to understand the problem and approach it. **Applications are built by layering one data model on top of another.** For each layer, the key question is how it is represented in terms of the next-lower layer:

1. Model real world problem into objects data structures, and interactions with APIs. 
2. Transferring data across network, with general purpose data model e.g. JSON, XML. tables in a relational DB or a graph model
3. Engineers who built DB, need to think a way of representing the JSON/XML/relationa/graph data in terms of bytes in memory, on disk, on a network
4. Even lower layer, hardware engineers needs to know how to represent bytes in terms of electrical currents, pulses of light, etc

This chapter will compare various data models.

## Relational Model vs. Document Model

### what is NoSQL

It has been retroactively reinterpreted as *Not Only SQL*. It is good for reasons:
1. easy to achieve greater scalability, handles large datasets, high write throughput
2. Specialised query operations that are not well supported by the relation models (*what are they??*)
3. Less restrictiveness of a relational schemas, more preferred for a dynamic and expressive data model (*with no schema restriction that enforces the data model during write operation, but it should still require the model mapping at application level*)

It has diverged into two main directions:
- Document databases: data comes in self-contained documents and *relationships between one document and another are RARE*
- Graph databases: On the opposite direction of Document DBs, that it suits for cases where anything is potentially related to everything!
### Impedance mismatch & Schema flexibility

Impedance mismatch means the disconnect between the models at application code and the model at database tables, rows, and columns. 

Object relational mapping (ORM) frameworks like *ActiveRecord* and *Hibernate* reduce the amount of boilerplate code required for this translation layer, but they can't completely hide the differences between the two models. 

Document oriented database like *MangoDB, RethinkDB, CouchDB, Espresso* supports storing data which self-container document as JSON. But it is hard be queried on its content.

Some developers feels having this JSON model reduces the *impedance mismatch* between the application code and the storage layer, as the application can decide how the data model should be exactly written in DB. **But, JSON as a data encoding format is problematic**(Chapter 4 will talk more on this).

JSON data model save all the required info in one place, with tree-like structure. e.g. a Linkedin profile can be saved as:

```json
{
	"user_id": 123,
	"first_name": "",
	"last_name": "",
	"positions" :[
		{"job_title": "sales", "organization": ""},	
		{"job_title": "senior sales", "organization": ""},	
	], 
	"educations": [
		{"school_name": "", "start": 1972, ...}	,
		{"school_name": "", "start": 1972, ...}	
	]
	...
}
```

### Document model limited on Many to One and Many to Many Relationships

Topics worth a separate discussion in **Part III** of this book:
- DB normalisation and denormalisation
- Caching

In contrast to relational DB, document DB can't deal with joining operations very well. Hence the joining logic normally is handled in application level, where multiple queries are issued in order to get all the required information and joined by application code. 

So which data model among the relational and document DB should be used? It depends on the *data in the application* - if it is document like / one-to-many tree like relationship, may use document model.

### Benefits of Document model

- Schema flexibility
- Data locality

Document model has *Schema flexibility* - *Schema-on-read* that is similar to runtime type checking in programming languages. Data structure is only get interpreted during the reading by application code. This is contract to the relational DB, which has this *schema-on-write* feature in addition. We prefer this document model when the nature of the underlying data has many different types, and it is not practical to put each type in its own table. 

A document is used stored as s single continuous string, encoded as JSON, XML, or binary variant e.g. MangoDB's BSON.  *Data Locality* means that the entire document is read and stored. It gives you the advantage when your query require the complete data info from the document. It it were a relational DB query, a few queries need to be issued and queried across the network and it causes the slowness of performance. 

### Convergence of document and relational database

Now relational database start support the storing of document (XML, JSON etc), and even provide the support of indexing, querying of XML/JSON data.

On the other hands, Document database also starts supporting relational-like joins in its query language. e.g. MangoDB, RethinkDB.

##  Query languages for Data

SQL as a *declarative query language*, in contrast to IMS, CODASYL which are *imperative code*.

Imperative language - tells teh computer to perform certain operations in a certain order. Stepping through the code line by line, evaluating conditions, updating variables, etc.

Declarative language - specify the pattern of data you want, how you want the data be transformed without caring how it is achieved. Achieving your command, is done by DB's *query optimiser*. Another benefit of declarative language is that engineers can improve the query logic without need to modify the query language. 

### MapReduce querying

MapReduce is a programming model for processing large amounts of *data in bulk, across many machines.*

MapReduce query is neither a declarative query language, nor a fully imperative query API, but somewhere in between. *The logic of the query is expressed with snippets of code, which are called repeatedly by the processing framework.*

Certain concepts on *map* and *reduce* function:
- Both map and reduce function is working on the data passed in as parameter, so they can't perform additional database queries

e.g. 
```
db.observations.mapReduce(
	function map() {

		var year = this.observationTimestamp.getFullYear(); var month = this.observationTimestamp.getMonth() + 1; emit(year + "-" + month, this.numAnimals);

	},  
	
	function reduce(key, values) {

		return Array.sum(values); 
	},

	{            
		query: { family: "Sharks" },
		out: "monthlySharkReport"
} );
```

## Graph like data models

Relational model can handle *simple cases of many-to-many relationships*, but more complex ones, we can model the data as *graph*.

Graph contains two kinds of objects:
- Vertices (nodes/entities)
- Edges (relationships/arcs)

e.g. Social graph where vertices are people, and the edges are the relationships (friends, families etc); or Web graph where the vertices are web pages, and edges are the hyperlinks in a page. 

### Property graphs

A vertex consists of:
- a unique identifier
- a set of outgoing edges
- a set of incoming edges
- a collection of key-value pairs which is the properties

Note that a vertex NOT necessarily has to represent a homogeneous data, they can represent objects of different types in a property graph.

A edge consists of:
- a unique identifier
- the vertex at which this edge starts (head)
- the vertex at which this dege ends (tail)
- a label to describe the relationship between the head and tail
- a collection of key-value pairs which is the properties

You can think of a graph store as consisting of two relational tables - one for vertices, the other for edges. 

```sql
CREATE TABLE vertices (  
    vertex_id integerPRIMARYKEY, properties json
    
    );
    
CREATE TABLE edges (  
    edge_id integer PRIMARY KEY,  
    tail_vertex integer REFERENCES vertices (vertex_id), head_vertex integer REFERENCES vertices (vertex_id), label text,  
    properties json
    
    );  
CREATE INDEX edges_tails ON edges (tail_vertex);
    
CREATE INDEX edges_heads ON edges (head_vertex);
```

Declarative and Imperative graph query languages
- Cypher query language
- Datalog


