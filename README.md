# Summary of GraphQL by Example
Summary of GraphQL by example with Mirko Nasato. The goal of this course is to learn the practical approach of GraphQL.

# What is GraphQL
GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

Some advantages with GraphQL :
- Ask for what you need, get exactly that
- Get many resources in a single request
- Describe what’s possible with a type system
- Move faster with powerful developer tools
- Evolve your API without versions
- Bring your own data and code

# Defining Schema
### GraphQL schema basics
Your GraphQL server uses a schema to describe the shape of your available data. This schema defines a hierarchy of types with fields that are populated from your back-end data stores. The schema also specifies exactly which queries and mutations are available for clients to execute.

The GraphQL specification defines a human-readable schema definition language (or SDL) that you use to define your schema and store it as a string.

Here's a short example schema that defines two object types: Book and Author:
```
type Book {
  title: String
  author: Author
}

type Author {
  name: String
  books: [Book]
}
```
A schema defines a collection of types and the relationships between those types. In the example schema above, a Book can have an associated author, and an Author can have a list of books.

Because these relationships are defined in a unified schema, client developers can see exactly what data is available and then request a specific subset of that data with a single optimized query.

Every type definition in a GraphQL schema belongs to one of the following categories:
- Scalar
- Object (This includes the three special root operation types: `Query`, `Mutation`, and `Subscription`.)
- Input
- Enum
- Union
- Interface

### GraphQL Resolver
Resolver is a collection of functions that generate response for a GraphQL query. In simple terms, a resolver acts as a GraphQL query handler. Every resolver function in a GraphQL schema accepts four positional arguments as given below
```
fieldName:(root, args, context, info) => { result }
```
Given below are the positional arguments and their description :
- root - The object that contains the result returned from the resolver on the parent field.
- args - An object with the arguments passed into the field in the query.
- context - This is an object shared by all resolvers in a particular query.
- info - It contains information about the execution state of the query, including the field name, path to the field from the root.

Resolvers in GraphQL can return different types of values as given below :
- null or undefined - this indicates the object could not be found
- array - this is only valid if the schema indicates that the result of a field should be a list
- promise - resolvers often do asynchronous actions like fetching from a database or backend API, so they can return promises
- scalar or object - a resolver can also return other values

# HTTP Request
Your GraphQL HTTP server should handle the HTTP GET and POST methods.
### GET request
When receiving an HTTP GET request, the GraphQL query should be specified in the "query" query string. For example, if we wanted to execute the following GraphQL query:
```
{
  me {
    name
  }
}
```
This request could be sent via an HTTP GET like so:
> http://myapi/graphql?query={me{name}}

### POST request
A standard GraphQL POST request should use the application/json content type, and include a JSON-encoded body of the following form:
```
{
  "query": "...",
  "operationName": "...",
  "variables": { "myVariable": "someValue", ... }
}
```
`operationName` and `variables` are optional fields. operationName is only required if multiple operations are present in the query. In addition to the above, we recommend supporting two additional cases:
- If the "query" query string parameter is present (as in the GET example above), it should be parsed and handled in the same way as the HTTP GET case.
- If the "application/graphql" Content-Type header is present, treat the HTTP POST body contents as the GraphQL query string.

### Response
Regardless of the method by which the query and variables were sent, the response should be returned in the body of the request in JSON format. As mentioned in the spec, a query might result in some data and some errors, and those should be returned in a JSON object of the form:
```
{
  "data": { ... },
  "errors": [ ... ]
}
```
### GraphQL Client
GraphQL client is code that makes a POST request to a GraphQL Server. In the body of the request we send a GraphQL query or mutation as well as some variables and we expect to get some JSON back. So really, the simplest GraphQL Client is curl.

### Apollo Server
Apollo Server is an open-source, spec-compliant GraphQL server that's compatible with any GraphQL client, including Apollo Client. It's the best way to build a production-ready, self-documenting GraphQL API that can use data from any source.

![Apollo Server](https://www.apollographql.com/docs/apollo-server/ee7fbac9c0ca5b1dd6aef886bb695e63/index-diagram.svg)
You can use Apollo Server as:
- A stand-alone GraphQL server, including in a serverless environment
- An add-on to your application's existing Node.js middleware (such as Express or Fastify)
- A gateway for a federated graph

Apollo Server provides:
- Straightforward setup, so your client developers can start fetching data quickly
- Incremental adoption, allowing you to add features as they're needed
- Universal compatibility with any data source, any build tool, and any GraphQL client
- Production readiness, enabling you to ship features faster
