## JSON Path
* The JSON path is a way to retrieve data from a specify JSON
* Given a file `vehicles.json` as below
```json
{
    "vehicles":{
        "car": {
            "color": "blue",
            "price": "$20,000",
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
        "price": "$20,000",
    }
]
```

* Given a JSON as below named `vechicles.json`
```json
[
    "car",
    "bus",
    "truck",
    "bike"
]
```
* Access the first element of the JSON
```bash
cat vechicles.json | jpath $[0]
```

* We will get the result the this one:
```json
[
    "car"
]
```

* To filter element in list we use the syntax
```
$[?( @ OPERATOR VALUE )]
```

* The `OPERATOR` can be in one on these values: `==`, `in`, `!=`, `>`, `<`, `>=`, `<=`,...
* The `@` is current item we are referencing
* For example in the above example if we need to filter for the "truck" in the list we can use the syntax as below.
```bash
cat vehicles.json | jpath $[?(@ == "truck" )]
```

## JSON Path wild card

* Given a json file named `file.json`
```json
{
    "car": {
        "color":"blue",
        "price": "$20.000"
    },
    "bus": {
        "color":"white",
        "price": "$120.000"
    }
}
```
* I would like to retrieve all colors from the json the result should be `["blue", "white"]`
```bash
cat file.json | jpath $.*.color
```

* Given JSON file named `test.json`
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

* To get car's wheel models
```bash
cat file.json | jpath $.car.wheels[*].model
```

* The result should return
```json
[
    "ABC",
    "BCD"
]
```

* To get all wheel models 
```bash
cat file.json | jpath $.*.wheels[*].model
```

* The result should return
```json
[
    "ABC",
    "BCD",
    "123",
    "456"
]
```
Further study can use this link: [JSON Path](https://github.com/json-path/jpath)