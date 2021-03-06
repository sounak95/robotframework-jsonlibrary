# robot_framework_json_library
JsonLibrary
Library scope:	global
Named arguments:	supported
Introduction
JsonLibrary is used for manipulating JSON object(dictionary)

You can get, add, update and delete your json object using JSONPath.

JSONPath Syntax
JSONPath	Description
$	the root object/element
@	the current object/element
. or []	child operator
..	recursive descent. JSONPath borrows this syntax from E4X
*	wildcard. All objects/element regardless their names.
[]	subscript operator. XPath uses it to iterate over element collections and for predicates. In Javascript and JSON it is the native array operator.
[,]	Union operator in XPath results in a combination of node sets. JSONPath allows alternate names or array indices as a set.
[start:end:step]	array slice operator borrowed from ES4
?()	applies a filter (script) expression.
()	script expression, using the underlying script engine.
Example of use
json_example.json

{
   "firstName": "John",
   "lastName": "doe",
   "age": 26,
   "gender": "male",
   "favoriteColor": [
       "blue", "black"
   ],
   "isMarried": false,
   "address": {
       "streetAddress": "naist street",
       "city": "Nara",
       "postalCode": "630-0192"
   },
   "officeaddress": {
       "streetAddress": "brooklyn street",
       "city": "newyork",
       "postalCode": "994-3332"
   },
   "phoneNumbers": [{
           "type": "iPhone",
           "number": "90-4567-8888"
       },
       {
           "type": "home",
           "number": "0123-4567-8910"
       }
   ]
}
Example Test Cases
* Settings *	
Library	JsonLibrary
* Test Cases *	
Add Json Object By JsonPath	
${json_data}  |  Load JSON From File  |  ../JSON/DataSetFiles/OpenAPIData/JSonFiles/example.json
${object_to_add}  |  Create Dictionary  |  latitude=13.1234  |  longitude=130.1234
${json_data}  |  Add Object To Json  |  ${json_data}  |  $..address  |  ${object_to_add}
Dictionary Should Contain Sub Dictionary  |  ${json_data['address']}  |  ${object_to_add}
${json_data}  |  Pretty Print Json  |  ${json_data}
 log  | ${json_data}  |
Result
   {
     "firstName": "John",
     "lastName": "doe",
     "age": 26,
     "gender": "male",
     "favoriteColor": [
       "blue",
       "black"
     ],
     "isMarried": false,
     "address": {
       "streetAddress": "naist street",
       "city": "Nara",
       "postalCode": "630-0192",
       "latitude": "13.1234",
       "longitude": "130.1234"
     },
     "officeaddress": {
       "streetAddress": "brooklyn street",
       "city": "newyork",
       "postalCode": "994-3332"
     },
     "phoneNumbers": [
       {
         "type": "iPhone",
         "number": "90-4567-8888"
       },
       {
         "type": "home",
         "number": "0123-4567-8910"
       }
     ]
   }
Shortcuts
Add Object To Json · Delete Object From Json · Get Json Elements · Get Value From Json · Json Element Should Exist · Json Element Should Not Exist · Json To String · Load Json From File · Pretty Print Json · Select Json Elements · Select Json Objects · String To Json · Update Json · Update Values To Json · Validate Jsonschema
Keywords
Keyword	Arguments	Documentation
Add Object To Json	json_data, json_path, object_to_add	
Adds a dictionary object to json_data using json_path

Arguments
json_data: json data as a dictionary object.
json_path: jsonpath expression.
object_to_add: dictionary object to add to json_data which is matched by json_path.
Returns
Returns a new json data object.

Example Test Cases
${json_data}  |  Load JSON From File  |  ../JSON/DataSetFiles/OpenAPIData/JSonFiles/example.json
&{dict} | Create Dictionary | latitude=13.1234 | longitude=130.1234 |
${json} | Add Object To Json | ${json_data} | $..address | ${dict} |
Delete Object From Json	json_data, json_path	
Deletes Object From Json data using json_path expression

Arguments
json_data: json data as a dictionary object.
json_path: jsonpath expression
Returns
Returns a new json_data

Example Test Cases
${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json	
${new_json_data}	Delete Object From Json	${json_data}	$..address.streetAddress
Get Json Elements	json_string, expr	
Get list of elements from json_string for matching expression.

Arguments
json_string : JSON string;
expr : JSONPath expression;
Returns
List of found elements or None if no elements were found

Example Test Cases
1. To Get Json Elements from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json	
${json_String_data}	Json To String	${json_data}	
@{json_elements}	Get Json Elements	${json_String_data}	$..number
=>

0123-4567-8910
Get Value From Json	json_data, json_path	
Gets Value From Json data using JSONPath

Arguments
json_data: json data as a dictionary object.
json_path: jsonpath expression.
Returns
Returns matched value from json in string format or None if no value were found

Example Test Cases
${json_data}  |  Load JSON From File  |  ../JSON/DataSetFiles/OpenAPIData/JSonFiles/example.json
${values}  |  Get Value From Json  | ${json_data} |  $..address.postalCode |
Should Be Equal As Strings  |  ${values}  |  "630-0192" |
Json Element Should Exist	json_string, expr	
Check the existence of one or more elements, matching expression.

Arguments
json_string : JSON string;
expr : JSONPath expression;
1. To Check Json Element Should Exist from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json
${json_String_data}	Json To String	${json_data}
Json Element Should Exist	${json_String_data}	.address.streetAddress:contains("naist street")
Json Element Should Not Exist	json_string, expr	
Check that one or more elements, matching expression, don't exist.

Arguments
json_string : JSON string;
expr : JSONPath expression;
1. To Check Json Element Should Exist from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json
${json_String_data}	Json To String	${json_data}
Json Element Should Exist	${json_String_data}	.address.streetAddress:contains("new york")
Json To String	source	
Serialize JSON structure into string.

Arguments
source : JSON structure
Returns
JSON string

Example Test Cases
1. To convert json to string from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json
${Json_String}    Json To String    ${json_data}
Load Json From File	file_name	
Loads Json data from file.

Arguments
file_name: Absolute json file name.
Returns
Returns json object as dictionary

Example Test Cases
${result}	Load Json From File	path/to/file.json
Pretty Print Json	json_string	
Returns formatted JSON string.

Arguments
json_string: JSON string.
Example Test Cases
${JsonString} Json To String {a:1,foo:[{b:2,c:3},{d:"baz",e:4}]} |${pretty_json} | Pretty Print Json | ${JsonString} |

Log	${pretty_json}
=>

{
   "a": 1,
   "foo": [
     {
       "c": 3,
       "b": 2
     },
     {
       "e": 4,
       "d": "baz"
     }
   ]
}
Select Json Elements	json_string, expr	
Returns list of elements from json_string with matching expression.

Arguments
json_string : JSON string;
expr : JSONPath expression;
Returns
List of found elements or None if no elements were found

Example Test Cases
1. To Select Json Elements from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json	
${json_String_data}	Json To String	${json_data}	
@{json_elements}	Select Json Elements	${json_String_data}	$..address
=>

"city" : "Nara" | "postalCode" : "630-0192"
Select Json Objects	json_string, expr	
Return list of elements from json_string, matching expression.

Arguments
json_string : JSON string;
expr : ObjectPath expression;
Returns
List of found elements. If no elements were found, empty list will be returned

1. To Select Json Object from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json	
${json_String_data}	Json To String	${json_data}	
@{json_elements}	Select Json Objects	${json_String_data}	$..number
Should Be Equal As Strings	@{Json_Objects}[1]	0123-4567-8910	
String To Json	source	
Deserialize string into JSON structure.

Arguments
source : JSON string
Returns
JSON structure

Example Test Cases
1. To convert string to json from a file by using "Get File" Builtin keyword to get json data.

Settings	Value	
Library	OperatingSystem	
${Json_String}	OperatingSystem.Get File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json
${json_data}	String To Json	${Json_String}
Update Json	json_string, expr, value, index=0	
Replace the value in the JSON string.

Arguments
json_string : JSON string;
expr : JSONPath expression for determining the value to be replaced;
value : the value to be replaced with;
index : index for selecting item within a match list, default value is 0;
Returns
Changed JSON in dictionary format.

Example Test Cases
1. To update value in json from a file using "Load JSON From File" keyword:

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json
${Json_String}    Json To String    ${json_data}
${json_update}  |  Update Json  |  ${Json_String}  |  $..address.city  |  Bangkok |
Update Values To Json	json_data, *params	
Updates value to json data. params needs to be passed using format jsonpath_expression=value.

Arguments
json_data: json data as a dictionary object.
-*params: Pass the data in the format of jsonpath_expression=value. It takes multiple params separated by Tab spaces. For example: "$..address.city=Bangkok"

Returns
Returns new json_object

Example Test Cases
1. To Update Json data with single value :

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json	
${Updated_json_data}	Update Values To Json	${json_data}	$..address.city=Bangkok
2. To Update Json data with multiple values :

${json_data}	Load JSON From File	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_example.json			
${Updated_json_data}	Update Values To Json	${json_data}	$..address.city=Bangkok	$..officeaddress.city=california	$..phoneNumbers[0].number=00001111
Validate Jsonschema	json_data, json_schema	
Validate JSON data according to schema.

Arguments
json_data: json data as a dictionary object.
input_schema: Pass the JSON Schema, which will be used to validates the structure of JSON data. This could be passed as string or filepath.
Example of use
Schema.json (Below is an example of json Schema, it defines the Structure for the json data)
   {
       "properties": {
           "name": {
               "type": "string"
           },
           "price": {
               "type": "number",
               "minimum": 0
           },
           "sku": {
               "description": "Stock Keeping Unit",
               "type": "integer"
           }
       },
       "required": ["name", "price"]
   }

The below example is a valid json data because according to the above Structure of "Schema.json", here "name" value type is a "string" and "price" value type is a "number"
   {
       "name": "eggs",
       "price": 80
   }

The below example is an invalid json data because according to the above Structure of "Schema.json", here the "price" value type is not a "number" but a "string"
   {
       "name": "eggs",
       "price": "80"
   }
Example Test Cases
1. To Validate JsonSchema:

Validate Jsonschema	${json_data}	${json_schema}
2. To Validate JsonSchema From File:

Validate Jsonschema	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_data.json	../JSON/DataSetFiles/OpenAPIData/JSonFiles/json_schema.json
Altogether 15 keywords. 
Generated by Libdoc on 2020-02-05 16:55:23.

