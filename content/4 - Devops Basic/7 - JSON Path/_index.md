---
title: "JSON Path"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

## JSON Path - Basic

- The JSON path is a way to retrieve data from a specify JSON
- Given a file `vehicles.json` as below

```json
{
  "vehicles": {
    "car": {
      "color": "blue",
      "price": "$20,000"
    },
    "bus": {
      "color": "white",
      "price": "$120,000"
    }
  }
}
```

```bash
cat vehicle.json | jpath $.vehicles.car
```

We will got the result like below

```json
[
  {
    "color": "blue",
    "price": "$20,000"
  }
]
```

- Given a JSON as below named `vechicles.json`

```json
["car", "bus", "truck", "bike"]
```

- Access the first element of the JSON

```bash
cat vechicles.json | jpath $[0]
```

- We will get the result the this one:

```json
["car"]
```

- To filter element in list we use the syntax

```
$[?( @ OPERATOR VALUE )]
```

- The `OPERATOR` can be in one on these values: `==`, `in`, `!=`, `>`, `<`, `>=`, `<=`,...
- The `@` is current item we are referencing
- For example in the above example if we need to filter for the "truck" in the list we can use the syntax as below.

```bash
cat vehicles.json | jpath $[?(@ == "truck" )]
```

## JSON Path - Wildcard

- Given a json file named `file.json`

```json
{
  "car": {
    "color": "blue",
    "price": "$20.000"
  },
  "bus": {
    "color": "white",
    "price": "$120.000"
  }
}
```

- I would like to retrieve all colors from the json the result should be `["blue", "white"]`

```bash
cat file.json | jpath $.*.color
```

- Given JSON file named `test.json`

```json
{
  "car": {
    "color": "blue",
    "price": "1",
    "wheels": [
      {
        "model": "ABC"
      },
      {
        "model": "BCD"
      }
    ]
  },
  "bus": {
    "color": "white",
    "price": "1",
    "wheels": [
      {
        "model": "123"
      },
      {
        "model": "456"
      }
    ]
  }
}
```

- To get car's wheel models

```bash
cat file.json | jpath $.car.wheels[*].model
```

- The result should return

```json
["ABC", "BCD"]
```

- To get all wheel models

```bash
cat file.json | jpath $.*.wheels[*].model
```

- The result should return

```json
["ABC", "BCD", "123", "456"]
```

## JSON Path - List
JSON Path is a query language for JSON, similar to XPath for XML. It allows you to navigate through elements and attributes in a JSON document. Here are some common JSON Path expressions:

- `$`: The root element to query. This can be omitted if the query starts from the root.
- `@`: The current node being processed by a filter predicate.
- `.` or `[]`: Child operator.
- `..`: Recursive descent. JSON Path will match all nodes at any depth.
- `*`: Wildcard. It can be used to match all elements in an object or array.
- `[]`: Subscript operator. It can be used to denote a specific element in an array.
- `[start:end]`: Array slice operator. It can be used to select a range of elements from an array.
- `[?(expression)]`: Filter expression. It can be used to filter elements based on a predicate expression.

Examples:
- `$.store.book[*].author`: Selects all authors of all books in the store.
- `$..author`: Selects all authors in the document.
- `$.store.*`: Selects all elements in the store object.
- `$.store..price`: Selects the price of all items in the store, including nested items.
- `$..book[2]`: Selects the third book.
- `$..book[-1:]`: Selects the last book.
- `$..book[0:2:1]`: Selects the first and second book. 1 is the step.
- `$..book[0:2]`: Selects the first two books.
- `$..book[?(@.isbn)]`: Selects books with an ISBN number.
- `$..book[?(@.price < 10)]`: Selects books with a price less than 10.


JSON Path makes it easy to extract specific data from JSON documents, making it a powerful tool for developers working with JSON data.


## Use case JSON Path with Kubectl

When working with kubectl, the cli tool show data in human readable format hide some information we need. If we want to extract data and use it in another tool, we can use JSON Path to extract the data.

```bash
kubectl get pods -o json
```

- The result should be like this
```json
{
  "items": [
    {
      "metadata": {
        "name": "pod-name"
      },
      "spec": {
        "containers": [
          {
            "name": "container-name"
          }
        ]
      }
    }
  ]
}
```
- Form JSON path we can extract the data we need

```bash
kubectl get pods -o=jsonpath='{.items[*].metadata.name}'
```
- The result should be like this

```
pod-name
```

- To get all container names

```bash
kubectl get pods -o=jsonpath='{.items[*].spec.containers[*].name}'
```

- The result should be like this

```
container-name
```

To combine multiple JSON Path 
```bash
kubectl get pods -o=jsonpath='{.items[*].metadata.name} {.items[*].spec.containers[*].name}'
```

- The result should be like this

```
pod-name container-name
```

- To display in a table format

```bash
kubectl get pods -o=jsonpath='{range .items[*]}
                                    {.metadata.name} {"\t"} {.spec.containers[*].name} {"\n"} 
                              {end}'
```

- The result should be like this

```
pod-name    container-name
```

- Print in a table format with column name using custom column
```bash
kubectl get node -o=custom-colums=<COLUMN_NAME>:<JSON_PATH>
```
* Example: 
```bash
kubectl get node -o=custom-columns=NAME:.metadata.name,IP:.status.addresses[?(@.type=="InternalIP")].address
```
- Columns are separated by comma
- The result should be like this

```
NAME    IP
node-1  10.0.0.1
```

- To sort the result by json path

```bash
kubectl get nodes --sort-by=.metadata.name
```

- The result should be like this

```
NAME    IP
node-1  10.0.0.1
node-2  10.0.0.2
```

- To sort descending

```bash
kubectl get nodes --sort-by=.metadata.name --sort-by=-
```

Further study can use this link: [JSON Path](https://github.com/json-path/jpath)
