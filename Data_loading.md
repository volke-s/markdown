# DATA LOADING

## Introduction and Concepts

Data loading is the process of data transfer from  

```tex
		1. Excel sheets
        2. Tables
```

 to databases. For these operation, we use a defined library the questionnaire library and the user derived Json layouts.

### Diagram

 In the questionnaire library we have developed three data loading methods there are as follows:

	a. Load: 
	b. Load_common:
	c. Load_user_inputs:

### Methods

A. Load

___



B. Load_common

___



C. Load_user_inputs

___

### Layouts

In the Json, they are two parts. They are as follows:-

	-  Table structure (as the source).
	-  Labels (as the destination) 

For more info go to [JSON_LAYOUTS](JSON_LAYOUTS)

## USAGE

Data loading supports the migration of data from one source to another to support many application requirements or needs. Firstly, it can be used where one needs to carry the below operations:-

- Data transfer from one database to another.  
- Transfer from simple data entry methods to a data management system. e.g database.
- To support application updates and upgrades.

### Data Transfer From Database To database

___

In this loading, the data transferred is either :- 

- one to one mapping - This is where data from transfer tables to the end database are a match thus loading labels are simple to extract and use.
- one to many mapping - This occurs where data references another column in a different table thus the user has to include the references in the source query to get the data and in the destination labels for the saving.

It allows the user to add and modify the data to fit the requirements thus efficiency and quality. 

### Transfer from simple data entry methods

___

Data stored in simple methods is hard to interrogate and its accumulation leads to the difficulty in management. Transfer of this to a database improves the data management as well quality. Thereby, Interrogating of the data becomes simple and can be done easily by anyone with good knowledge in using the language SQL. 

SQL allows manipulation of data through a number of statements . They are:- 

- Create - Allows a user to create a new attributes to a database.
- Select - Allows a user to extract data in a manner that is specified.
- Insert - Allows a user to add new data into the already existing.
- Delete - Allows a user to remove unwanted data from the existing.

How our library works.

### To support application updates and upgrades

___

For data to maintain its integrity, transfer is key. This is needed where one needs to add or remove certain elements or attributes from an already existing database without altering its model and data. Thus, a user needs to design a new database with the new requirements and afterwards transfer the data adding the new information.

This is crucial where if any problem is to occur during the changes implementation, one can revert to the older version without any data loss or corruption.

## DESCRIPTION

To transfer data, we develop json based layouts that help in querying of the data to load. These requires a *source* and a *destination*. It can be used for loading both small amounts of data and large amounts regardless of the source of the data.

The source is defined as a table which comprises of a few parameters. The syntax varies from the type of data entry system. Firstly, we'll have a look at the tabular entry.

## JSON LAYOUTS

The json files have used for loading data to databases through the questionnaire library.

### TABLE

___

This is data stored in simple tables. For tabular, we have two main parameters. They are as follows:

 1. Class name -  This is the type of query to execute.

    ```
    "class_name": "mutall\\capture\\query",
    ```

    

 2. Args - this are the parameters required by the class name to be able to query for the data. They are:-

    - Tname - The table name to look up for the data.
    - Query - The sql statement to execute on the table above to get the data.
    - Database - The database in which the table is and for getting the data.

```
        "args": [
            "tname",
            "query",
            "database"
        ]
```

### EXCEL SHEETS

___

This is data stored in formatted sheets in form of csv *(comma separated values)*. It requires two parameters as well. They are as follows:-

​	1. Class name - The type of query to execute.

```
	"class_name": "mutall\\capture\\csv",
```

 2. Args - this are the arguments required to look up for data from the excel sheets. They are as follows:-

    -  Tname -The name of the text table.
    -  Filename - The filename that holds the data.
    -  Cnames - These are the column names where if left empty, the user intends to use the default values as in the data sheet.
    -  Delimiter - This is the operator that is used to separate the data. i.e comma ','.
    -  Header start - Represents the row number which by default starts from 0, where if the number is a negative value, means that the file has no header.
    -  Body start - Represents the row number starting from a value of 0 This indicates where the body of the data starts.
    
    ```json
            "args":[
                "tname", 
                "filename",
                [cnames],
                "delimiter",  
                header_start,
                body_start
             ]
    ```

### LABELS

___

 The labels contain the destination of the data. It has 5 parameters with the following structure :-

 	[string, string, [ ], string, basic_value]

 The above represents different values required by the questionnaire in order to load the data. They are as follows:-

​		*1.This contains the database name of where to save the data.*

​		2.*The table name in the database to insert the data.*

​		3.*The alias which is empty when loading to a specific table only. Its however takes the name of the table provided to aid in successful loading of foreign key data.*

​		4.*The column name where the data is to be inserted in the specified table.*

​		5.*The value to save. This may be a simple value or a look up value which is queried from the provided source.*

NB: For look up data, the data is referenced using the structure below.

​	*[ class_name, tname, cname]* 

- The class name is the type of data querying to be performed 	which is the look up from a referenced table.
- The tname is the table to look for the requested data.
- The cname is the column where the data is stored.

For a column name(cname), it varies from one layout to the  other i.e the value in:- 

 - excel is referenced by a number relative to its position starting from 0 e.g.   

```json
["\\capture\\lookup","t1", 0]
```

  - simple tables its referenced by the specific column name e.g.

```json
["\\capture\\lookup","client","name"]
```

This mode of loading can be used to load either a single table a  time or multiple tables at the same time.

### Example of a JSON layout

A simple JSON layout for loading a:- 

​	I. single table.

```json
[ {
        "class_name": "capture\\query",
        "args": [
            "msg",
            "select id, body, date from msg",
            "mutallco_rental"
        ]
   },
   ["rentize","msg", [],"id", ["\\capture\\lookup","msg","id"]],
   ["rentize","msg", [],"body", ["\\capture\\lookup","msg","body"]],
   ["rentize","msg", [],"date", ["\\capture\\lookup","msg","date"]]
]
```

​	II. Foreign table

```json
{
        "class_name": "capture\\query",
        "args": [
            "agreement",
            "select client.name as client_name,room.uid as room_uid, agreement.amount,agreement.start_date,agreement.duration,agreement.review,agreement.terminated,agreement.valid,agreement.comment from agreement inner join client on agreement = client.client inner join room on agreement = room.room",
            "mutallco_rental"
        ]
   },
["rentize","agreement", ["agreement"],"start_date", ["\\capture\\lookup","agreement","start_date"]],
["rentize","agreement", ["agreement"],"end_date", ["\\capture\\lookup","agreement","terminated"]],
["rentize","agreement", ["agreement"],"review", ["\\capture\\lookup","agreement","review"]],
["rentize","agreement", ["agreement"],"amount", ["\\capture\\lookup","agreement","amount"]],
["rentize","agreement", ["agreement"],"is_valid", ["\\capture\\lookup","agreement","valid"]],
["rentize","agreement", ["agreement"],"duration", ["\\capture\\lookup","agreement","duration"]],
["rentize","agreement", ["agreement"],"comment", 	["\\capture\\lookup","agreement","comment"]],
   
["rentize","tenant",["agreement"],"name",["\\capture\\lookup","agreement", "client_name"]],
["rentize","room", ["agreement"], "uid", ["\\capture\\lookup", "agreement", "room_uid"]],
```

​	III. excel sheet(csv).

```json
[
    ["school", "grade", [], "name", "8"],
    ["school", "stream", [], "name", "Red"],
    ["school", "stream", [], "id", "r"],
    ["school", "exam", [], "name", "Targeter Exam"],
    
    ["school", "stage", [], "year", "2021"],  
    ["school", "school", [], "id", "KAPS"],
    ["school", "school", [], "name", "Kiserian Adventist Primary School"],
    ["school", "progress", [], "progress", null],
    
    {
        "class_name":"\\capture\\csv",
        "args":[
            "t1", 
            "/mutall_projects/school/v/data/loadables/scores/CLASS 8 RED TARGETER.csv",
            [],
            ",",  
            -1,
            4
        ]
    },
    ["school", "student", [], "name", ["capture\\lookup","t1", 0]],
    ["school", "student", [], "gender", ["capture\\lookup","t1", 1]],
    
    ["school", "performance", ["eng"], "out_of", 50],
    ["school", "sitting", ["eng"], "date", "2021-2-10"],
    ["school", "subject", ["eng"], "id", "eng"],
    ["school", "subject", ["eng"], "name", "English"],
    ["school", "score", ["eng"], "value", ["capture\\lookup","t1", 2]],
    
    ["school", "performance", ["comp"], "out_of", 40],
    ["school", "sitting", ["comp"], "date", "2021-2-10"],
    ["school", "subject", ["comp"], "id", "comp"],
    ["school", "subject", ["comp"], "name", "composition"],
    ["school", "score", ["comp"], "value", ["capture\\lookup","t1", 3]],
    ["school", "score", ["eng"], "percent", ["capture\\lookup","t1", 4]],
   
    ["school", "performance", ["kis"], "out_of", 50],
    ["school", "sitting", ["kis"], "date", "2021-2-10"],
    
    ["school", "subject", ["kis"], "id", "kiswa"],
    ["school", "subject", ["kis"], "name", "Kiswahili"],
    ["school", "score", ["kis"], "value", ["capture\\lookup","t1", 5]],
    
    ["school", "performance", ["ins"], "out_of", 40 ],
    ["school", "sitting", ["ins"], "date", "2021-2-10"],
    
    ["school", "subject", ["ins"], "id", "ins"],
    ["school", "subject", ["ins"], "name", "insha"],
    ["school", "score", ["ins"], "value", ["capture\\lookup","t1", 6]],
    ["school", "score", ["kis"], "percent", ["capture\\lookup","t1", 7]],
    
    ["school", "performance", ["math"], "out_of",100],
    ["school", "sitting", ["math"], "date", "2021-2-10"],
    
    ["school", "subject", ["maths"], "id", "maths"],
    ["school", "subject", ["maths"], "name", "Mathematics"],
    ["school", "score", ["maths"], "value", ["capture\\lookup","t1", 8]],
    
    ["school", "performance", ["sci"], "out_of",100],
    ["school", "sitting", ["sci"], "date", "2021-2-10"]
]
```

# Data Extraction

### Simple Attributes

### Identifier Attributes

### Foreign key Attributes

### SQL

## Data Ingestion
