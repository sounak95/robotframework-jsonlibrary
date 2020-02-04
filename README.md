# robotframework-jsonlibrary

JsonLibrary is used for manipulating JSON object(dictionary)

        You can get, add, update and delete your json object using JSONPath.

        == JSONPath Syntax ==
        | JSONPath | Description |
        | $        | the root object/element |
        | @        | the current object/element |
        | . or []  | child operator |
        | ..       | recursive descent. JSONPath borrows this syntax from E4X |
        | *        | wildcard. All objects/element regardless their names. |
        | []       | subscript operator. XPath uses it to iterate over element collections and for predicates. In Javascript and JSON it is the native array operator. |
        | [,]      | Union operator in XPath results in a combination of node sets. JSONPath allows alternate names or array indices as a set. |
        | [start:end:step] | array slice operator borrowed from ES4 |
        | ?()      | applies a filter (script) expression. |
        | ()       | script expression, using the underlying script engine. |

        == Example of use ==
        json_example.json

        | {
        |    "firstName": "John",
        |    "lastName": "doe",
        |    "age": 26,
        |    "gender": "male",
        |    "favoriteColor": [
        |        "blue", "black"
        |    ],
        |    "isMarried": false,
        |    "address": {
        |        "streetAddress": "naist street",
        |        "city": "Nara",
        |        "postalCode": "630-0192"
        |    },
        |    "officeaddress": {
        |        "streetAddress": "brooklyn street",
        |        "city": "newyork",
        |        "postalCode": "994-3332"
        |    },
        |    "phoneNumbers": [{
        |            "type": "iPhone",
        |            "number": "90-4567-8888"
        |        },
        |        {
        |            "type": "home",
        |            "number": "0123-4567-8910"
        |        }
        |    ]
        | }

        == Example Test Cases ==
        | *** Settings ***     |
        | Library              | JsonLibrary |
        |                      |
        | *** Test Cases ***   |
        | Add Json Object By JsonPath |

        | ${json_data}  |  Load JSON From File  |  ../JSON/DataSetFiles/OpenAPIData/JSonFiles/example.json
        | ${object_to_add}  |  Create Dictionary  |  latitude=13.1234  |  longitude=130.1234
        | ${json_data}  |  Add Object To Json  |  ${json_data}  |  $..address  |  ${object_to_add}
        | Dictionary Should Contain Sub Dictionary  |  ${json_data['address']}  |  ${object_to_add}
        | ${json_data}  |  Pretty Print Json  |  ${json_data}
        |  log  | ${json_data}  |

        == Result ==

        |    {
        |      "firstName": "John",
        |      "lastName": "doe",
        |      "age": 26,
        |      "gender": "male",
        |      "favoriteColor": [
        |        "blue",
        |        "black"
        |      ],
        |      "isMarried": false,
        |      "address": {
        |        "streetAddress": "naist street",
        |        "city": "Nara",
        |        "postalCode": "630-0192",
        |        "latitude": "13.1234",
        |        "longitude": "130.1234"
        |      },
        |      "officeaddress": {
        |        "streetAddress": "brooklyn street",
        |        "city": "newyork",
        |        "postalCode": "994-3332"
        |      },
        |      "phoneNumbers": [
        |        {
        |          "type": "iPhone",
        |          "number": "90-4567-8888"
        |        },
        |        {
        |          "type": "home",
        |          "number": "0123-4567-8910"
        |        }
        |      ]
        |    }
