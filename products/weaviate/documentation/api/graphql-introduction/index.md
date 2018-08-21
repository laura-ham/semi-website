---
layout: article-start
title: GraphQL introduction
description: Explanation of a GraphQL Local function.
topic: API
tags: ['weaviate', 'API', 'GraphQL']
video-link: 
video-caption: 
menu-order: 2
open-graph-type: article
---

## Index
- [What is GraphQL](#what-is-graphql)
- [How to use GraphQL](#how-to-use-graphql)

## What is GraphQL

GraphQL is a query language which allows clients to ask and get exactly what they need. GraphQL is not meant to be a database language, but can be used for many databases by different database languages and connectors. Instead, GraphQL gives you as a user nothing more and nothing less than what you need. This allows the user to control the data, which makes developers' life easy and ensures apps on top of GraphQL to be fast and stable. You can learn more about what GraphQL is [here](https://graphql.org/).

Because GraphQL APIs are organised in schemas with types and fields rather than endpoints, it makes is very suitable for querying data in a knowledge network based on schemas, just like Weaviate.


## How to use GraphQL

To query Weaviate, the most easy way is to use GraphQL. The endpoint for GraphQL is always the same:

```bash
$ curl -X POST -H "X-API-KEY: [[apiKey]]" -H "X-API-TOKEN: [[apiToken]]" -H "Content-Type: application/json" --data '[[DATA]]' "https://weaviate-host/weaviate/v1/graphql"
```

The body should contain the GraphQL query. The body of the following example will return all animal names in your local Weaviate instance.
```graphql
{
  Local{
    Get{
      Things{
        Animal{
          name
        }
      }
    }
  }
}
```

### How to write queries

