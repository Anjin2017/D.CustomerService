Sample - Customer Service APIs

Overview:
This API specification is designed for customer service support. First version is targeted for initial release with API to access customers object.

Further extension is defined with Products and Orders

Version V1 APIs

Get /Customers/{limit}{offset}
   -> this API is used to get full list of customers , call to this API should be initiated using limit and offset query parameters. 
   if there are no more records to return based on limit and offset, API will retutn 404(Not Found) status code.
      
Get 	/Customers/{ID}
    -> Get customer details using customer ID
   
Post	/Customers
    =>	Create Customer record. Address is maintained as separate resource. When Customer is created, Address is created as well.               Implementation of association between customer and address object abstracted from API consumers.
Patch	/Customers
    
Put 	/Customers


Extension   V2 APIS

Get /Orders/{Limit}{offset}
    -> this api is used to get all sales orders. this API should be called using Limit and Offset query parameters.  
       if there are no more records to return based on limit and offset, API will return 404(Not Found) status code.
Get  /Orders?customerId=<Customer id>&ProductId=<Prodcut id>
   ->  Get list of orders by customer ID, along with customer ID, pagination query parametes required 

Get  /Orders?ProductId=<Prodcut id>
   -> Gey list of Order by Product Id. Pagination parameters required
    
   
Get /Order/OrderDate/{FromDate}{ToData}
    ->  This API will be used to get order using order date range

Get /Products/{limit)(offset)
    ->  Get all Products. this API need pagination parameters to limit the response size.

Get	 /Products/{ID}
    ->  Get Products by ID
    
Post /Products

Put	 /Products


1.    A consumer may periodically (every 5 minutes) consume the API to enable it (the consumer) to maintain a copy of the provider API's customers (the API represents the system of record)

Response cache:
    This API Specification enforces header attributes to control response caching. Response header should include Cache-Control attribute. Using cache control attribute max-age of the response is defined. The response also contains ETag to validate the response expiry.  With this client will not initiate new request till max-age is elapsed.

2.    A mobile application used by customer service representatives that uses the API to retrieve and update the customer's details
Minimizing Network Roundtrip and Payload size:
This API specification requires the client to send “if-none-match” attribute in request header along with ETag value from the previous response.  In response, API will check if there are any changes done to the resource. if no change was done API will return status code "304 – No Changes". Using this status code, the client application can continue using previous response and avoid transmitting response payload over the network.



3.    Simple extension of the API to support future resources such as orders and products

Extension V2
This API specification includes RAML extension with APIs for additional resources. Orders and Products APIs are defined in this extension. Using this version of API Orders can be queried using Order ID, Order ID. Extension version support sales order query by customer id and product id.


Key Design Decisions:

1.	Customer Address is considered as a separate resource. Customer objects are composite of customer attributes and Address object. By using Address as separate resource further extension can be done to include API for operations on Address. Some of the future extension can include Address Validation API, CRUD on Address 
2.	Delete is allowed through customer ID. This will ensure accidental deletion of customer data
3.	GET methos on Customers and Orders required pagination parameters(Limit and Offset). 


Alternative Design:

2.	API for searching by FirstName and LastName can be provided. Current API spec provide provision to use these parameters as query parameters.
3.	Currently the resource properties are limited and payload size is relatively less. Otherwise explicit properties can be mentioned to limit the payload size. Also, in current spec caching is enabled for the APIs returning large data, so network load is relatively less.


Improvements Identified:
1.	Spec includes a sample set of error codes. all possible error conditions are required in the improved spec.
2.	Code refactoring and reusable codebase using Libraries, Traits, Resource Types, and Overlays.
3.	Detailed documentation can be another improvement area in this spec.
