# DataObject

This is a simple set of methods providing extended JSON/c-object referencing capabilites for a 4D database. 

## Installing
The methods are provided as text files. To install them you can download the repo and import them into your database or create the methods and paste in the text. No plugins or components are required. 

The funciton OB_new is a method written as
    `$0:=JSON parse("{}")`
Add it as well or use whatever method you currently have.

## Uses
dataObj_get_dataElement is the real workhorse here. It's called as follows:

  ` $obj:=dataObj_get_dataElement (text{;boolean{;c-obj}}) `

    $1 is a dot-notation path

    $2 is TRUE to build out the data obj -- default is TRUE  

    $3 is the c-obj                      -- default is prosObject

    $0 is ref to the data object
 ### path
 $1 is a dot-notation path. This can be simple:
 
    name
    
 or complex including array references:
 
    order.agents[2].name
 ### the build option
 $2 is an optional boolean parameter to control whether the data object will be built out or not. When this option is TRUE the data object will be expanded to accomodate the path. Objects and arrays will be added as required. 
 
 When this option is FALSE the data object won't be altered. If the path doesn't exist nothing is returned. 
 ### the data object
 The default data object (4D c-object) is a process variable named `prosObject`. It's created and initialized if it doesn't already exist. 
 
 Optionally you can specify a specific object with $3. 
 ### the return object
 dataObj_get_dataElement returns an object - not a specific value. You can read from or write to that object. For example, if I want to update the data object I could do this:
 
 ```$obj:=dataObj_get_dataElement ("order.agents[2].partner.addresses[5].adrsText")  
 
   OB SET($obj;"singleLine";"123 main street")  
   ```
 
 The default data object looks like this:  
 {
    "order": {
        "agents": [
            null,
            {
                "partner": {  
                    "addresses": [  
                        null,  
                        null,  
                        null,  
                        null,  
                        {  
                            "adrsText": {  
                                "singleLine": "123 main street"  
                            }  
                        }  
                    ]  
                }  
            }  
        ]  
    }  
}    
 
 
 
 
 I can get the first name like so:
 
 `$obj:=dataObj_get_dataElement ("order.agents[2].name")
 
 $0:=OB get($obj;"last")`
 

 
