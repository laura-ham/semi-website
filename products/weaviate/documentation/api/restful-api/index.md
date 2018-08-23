---
layout: article-start
title: RESTful API
description: How to use the RESTful API
topic: API
tags: ['weaviate', 'API']
video-link: 
video-caption: 
menu-order: 2
open-graph-type: article
---

Weaviate can be accessed directly through the RESTful APIs. This is handy to create or update single Things or Actions. In case you want to crawl the graph, it is advised to use the [GraphQL endpoint](#).

[Swagger](https://swagger.io) is used to create the RESTful API documentation.

[Full Open API docs]().

<!-- markdown-swagger -->
 Endpoint                      | Method | Auth? | Description                                                                                                                                                            
 ----------------------------- | ------ | ----- | -----------------------------------------------------------------------------------------------------------------------------------------------------------------------
 `/actions`                    | POST   | No    | Registers a new action. Given meta-data and schema values are validated.                                                                                               
 `/actions/validate`           | POST   | No    | Validate an action's schema and meta-data. It has to be based on a schema, which is related to the given action to be accepted by this validation.                     
 `/actions/{actionId}`         | DELETE | No    | Deletes an action from the system.                                                                                                                                     
 `/actions/{actionId}`         | GET    | No    | Lists actions.                                                                                                                                                         
 `/actions/{actionId}`         | PATCH  | No    | Updates an action. This method supports patch semantics. Given meta-data and schema values are validated. LastUpdateTime is set to the time this function is called.   
 `/actions/{actionId}`         | PUT    | No    | Updates an action's data. Given meta-data and schema values are validated. LastUpdateTime is set to the time this function is called.                                  
 `/actions/{actionId}/history` | GET    | No    | Returns a particular action history.                                                                                                                                   
 `/graphql`                    | POST   | No    | Get an object based on GraphQL                                                                                                                                         
 `/keys`                       | POST   | No    | Creates a new key. Input expiration date is validated on being in the future and not longer than parent expiration date.                                               
 `/keys/me`                    | GET    | No    | Get the key-information of the key used.                                                                                                                               
 `/keys/me/children`           | GET    | No    | Get children of used key, only one step deep. A child can have children of its own.                                                                                    
 `/keys/{keyId}`               | DELETE | No    | Deletes a key. Only parent or self is allowed to delete key. When you delete a key, all its children will be deleted as well.                                          
 `/keys/{keyId}`               | GET    | No    | Get a key.                                                                                                                                                             
 `/keys/{keyId}/children`      | GET    | No    | Get children of a key, only one step deep. A child can have children of its own.                                                                                       
 `/keys/{keyId}/renew-token`   | PUT    | No    | Renews the related key. Validates being lower in tree than given key. Can not renew itself, unless being parent.                                                       
 `/meta`                       | GET    | No    | Gives meta information about the server and can be used to provide information to another Weaviate instance that wants to interact with the current instance.          
 `/peers`                      | POST   | No    | Announce a new peer, authentication not needed (all peers are allowed to try and connect). This endpoint will only be used in M2M communications.                      
 `/peers/answers/{answerId}`   | POST   | No    | Receive an answer based on a question from a peer in the network.                                                                                                      
 `/peers/echo`                 | GET    | No    | Check if a peer is alive.                                                                                                                                              
 `/peers/questions`            | POST   | No    | Receive a question from a peer in the network.                                                                                                                         
 `/things`                     | GET    | No    | Lists all things in reverse order of creation, owned by the user that belongs to the used token.                                                                       
 `/things`                     | POST   | No    | Registers a new thing. Given meta-data and schema values are validated.                                                                                                
 `/things/validate`            | POST   | No    | Validate a thing's schema and meta-data. It has to be based on a schema, which is related to the given Thing to be accepted by this validation.                        
 `/things/{thingId}`           | DELETE | No    | Deletes a thing from the system. All actions pointing to this thing, where the thing is the object of the action, are also being deleted.                              
 `/things/{thingId}`           | GET    | No    | Returns a particular thing data.                                                                                                                                       
 `/things/{thingId}`           | PATCH  | No    | Updates a thing data. This method supports patch semantics. Given meta-data and schema values are validated. LastUpdateTime is set to the time this function is called.
 `/things/{thingId}`           | PUT    | No    | Updates a thing data. Given meta-data and schema values are validated. LastUpdateTime is set to the time this function is called.                                      
 `/things/{thingId}/actions`   | GET    | No    | Lists all actions in reverse order of creation, related to the thing that belongs to the used thingId.                                                                 
 `/things/{thingId}/history`   | GET    | No    | Returns a particular thing history.                                                                                                                                    
<!-- /markdown-swagger -->

<sup>Mardown generated with `markdown-swagger OpenAPI-Specification/schema.json README.md`</sup>