---
alias: nae4oth9ka
path: /docs/reference/simple-api/custom-queries
layout: REFERENCE
shorttitle: Custom Queries
beta: true
description: Queries for custom use cases can be added to your GraphQL schema using schema extension.
simple_relay_twin: shae0xie0e
tags:
  - simple-api
related:
  further:
  more:
---

# Custom Queries in the Simple API

For use cases that are not covered by the automatically generated CRUD-style API, [Schema Extension Functions](!alias-xohbu7uf2e) can be used to enhance your GraphQL schema with custom queries.

You can define the **name, input arguments and payload of the query** and **resolve it with a Graphcool Function**.

## Example

> Validate the age of a user

Schema Extension SDL document:

```graphql
type AgePayload {
  isValid: Boolean!
  age: Int!
}

extend type Query {
  isValidAge(age: Int!): AgePayload
}
```

Graphcool Function:

```js
module.exports = function age(event) {
  const age = event.data.age

  if (age < 0) {
    return {
      error: "Invalid input"
    }
  }

  const isValid = age >= 18

  return {
    data: {
      isValid,
      age
    }
  }
}
```

Then the query can be called like this using the Simple API:

```graphql
query {
  isValidAge(age: 12) {
    isValid # false
    age # 12
  }
}
```

Note that the returned object contains a `data` key, which in turn contains the `age` field that was specified in the `AgePayload` in the SDL document. [Error handling](!alias-quawa7aed0) works similarly to other Graphcool Functions, if an object containing the `error` key is returned.
