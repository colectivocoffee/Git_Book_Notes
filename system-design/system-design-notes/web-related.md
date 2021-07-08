# Web Related

## API Model for HTTP Transfer

### RPC vs REST

**What are RPC and REST?**  
RPC and REST are API models when we build APIs. 

**When to use RPC and REST?**  
When building endpoints or APIs, there are two primary models for API design: RPC and REST. 

* **Remote Procedure Call \(RPC\)**: Using procedures to transfer data. Data transferred are binary-type, which is machine-readable. Faster but hard to understand by humans.
* **Representational State Transfer \(REST\)**: Using pre-defined states such as GET/POST/DELETE to send data when we specify URLs. Data transferred are entity-oriented, which is human readable. Slower but easy to understand by humans.

There are three significant and distinct approaches for building APIs that use HTTP, they are    
\(1\) gRPC \(e.g. Apache Thrift, Avro...\)  
\(2\) REST \(e.g. JSON\)  
\(3\) OpenAPI

* **RPC**

  For RPC, the entities are procedures, and the data is hidden behind the procedures.

  * **gRPC**  
    gRPC is based on the RPC model. gRPC is a technology for RPC API implementation using HTTP 2.0, but HTTP is not exposed to the API designer. gRPC-generated stubs and skeletons hide HTTP from the client and server too, so we don't have to worry how RPC is mapped to HTTP. Below are the three steps to use gRPC API:

    * Decide which procedure to call
    * Calculate the parameter values to use \(if any\)
    * Use a code-generated stub to make the call, passing the parameter values

  
    Benefits of gRPC:  
    Disadvantages of gRPC:

* **REST** The most common API model is REST, REST is an entity-oriented model. Which is simpler, more regular, easier to understand, and more stable than simple RPC models.   Plus, the entity-oriented model provides an overall organization for the system behaviors. Let's say we are having an online shopping cart, we are able to visualize products, carts, orders, accounts for this online shopping cart. On the other side, if we use RPC procedures, it would result in a long, unstructured list of procedures for adding products to carts, checking out, returning products, and others.

### Conclusion 

RPC-style endpoints are great when you want only one job done well. Meaning useful for one or two app clients \(Great for internal service use cases\). This adds simplicity and clarity to the service.  
\(1\) Faster to transfer, because it is binary  
\(2\) Great for internal services that just have one or two clients requesting data  
\(3\) Suitable for monolithic system   
\(4\) MVC systems with a basic CRUD can benefit from RPC when there is little need to scale.

For REST endpoint, it provides domain data, which becomes helpful when segregating data into different domains. This makes it useful when you have any number of apps requesting data.  
\(1\) Slower to transfer, because it has to send field names in text.  
\(2\) Great for distributed large scale system that many apps are requesting data  
\(3\) Suitable for distributed system

#### Reference

1. gRPC and REST: [https://cloud.google.com/blog/products/api-management/understanding-grpc-openapi-and-rest-and-when-to-use-them](https://cloud.google.com/blog/products/api-management/understanding-grpc-openapi-and-rest-and-when-to-use-them)



