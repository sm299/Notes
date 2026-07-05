# RESTful API

*REST -> Representational State Transfer*

### Nesting ->
Nesting is a way of organizing related resources in an API by placing one resource inside another in the URL. It shows the relationship between the resources.This one accesses related resources through the URL path means it uses @PathVariable<br>
It's like relations, means suppose an api is like "/orders/{id}/items", it will fetch what are the items of that particular OrderId.<br>


### Filtering ->
Filtering is the process of returning only the data that matches specific conditions by using query parameters in the API request. This one limits results using query parameters means it uses @RequestParam.<br>
It's the type, means if the api is like "/orders?status=completed", it will give all the orders which are completed, i.e. the type of orders.<br>