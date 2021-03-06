# snrest
Javascript wrapper for calling ServiceNow REST API.
Acts as an alternative for simple GlideAjax requests, reuquiering no server side code (script include).

## Usage
### Generate URL
to build the URL string use function 
```javascript
buildRequestString(inpObj)
```

### Execute request
To do a actual call to the current instance use restRequest(inpObj, callback)

Example:
```javascript
restRequest({"table":"change_request","limit":2,"query":"active=true"}, function(resp){ 
    console.log(resp)
});
```
This will dump the last 2 changes as JSON to the console.
Documentation will be expanded soon...

### inpObj definition
| Key  		  |Shortcut | Type   |Default              | Description                                                   |
|-----------|---------| ------ |---------------------|---------------------------------------------------------------|
| url  		  |u        | string | ""                  | Instance base url default empty when used in current instance |
| api  		  |a        | string | "api/now/table/"   | Endpoint of API to use (currently only table API is supported)                                        |
| table  		|t        | string | "incident"          | Table or DB view to perform the operation on                  |
| fields    |f        | string | ""                  | A comma-separated list of fields to return in the response (all when empty)|
| view      |v        | string | ""                    | Render the response according to the specified UI view (overridden by fields)|
| query  		|q        | string | "sys_id={0}"                  | An encoded query string used to filter the results            |
| values    |vals        | array  | []                  | Values to replace in query ex: ['yes','maybe'] will replace<br />  answer={0}^answer={1} to answer=yes^answer=maybe|
| order  		|o        | string | "!sys_updated_on"   | Comma separated list of fields to order results by, to order descendant, preceed the field with a ! sign.|
| limit     |l        | int    | 10                  | The maximum number of results returned per page               |
| displayValue|d      | string | all               | Return the display value (true), actual value (false), or both (all) for reference fields|                 
| excludeReferenceLink|e| bool | true                | true to exclude Table API links for reference fields          |
| suppressPaginationHeader|s| bool | true            | true to supress pagination header          |


### Usage example
The following example can be used in a client script. 
This example can be used to set the location of an incident, after a CI is selected, based on the location of this CI.

```javascript
restRequest({"t":"cmdb_ci","vals":[newValue],"f":"location"},
	function(resp){
		var loc = resp[0].location;
		g_form.setValue('location',loc.value,loc.display_value);
});
```

To use this the script has to be available as a global UI script.
Benefit is you only need a few lines of client-side code, no server-side Script Include AbstractAjaxProcessor is needed :)






