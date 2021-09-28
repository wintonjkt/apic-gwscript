# apic-gwscript  
  
### Sample code to insert an item to response body:  
```
//get the runtime API context
var json = apim.getvariable('message');
console.info("json %s", JSON.stringify(json));

//add a new attribute to the payload body
json.body.platform = 'Powered by IBM API Connect';

//set the runtime API context
apim.setvariable('message.body', json.body);

```
  
The message context variable is the default context that contains the API runtime context, which includes the JSON body sent back to the consumer.  
```
request: pre-built context variable provides the original request message  
message: pre-built context variable provides access to the current message in the assembly  
custom: context variables created during the API assembly with unique names and used in subsequent actions, such as GatewayScript.  
Each context variable has additional attributes such as body, headers, etc ... that provide additional runtime context.  
```    
Inject an HTTP response header in the same GatewayScript policy. Replace the existing code with the following:  
```  
//get the runtime API context
var json = apim.getvariable('message');
console.error("json %s", JSON.stringify(json.headers));

//add a new attribute to the payload body 
json.body.platform = 'Powered by IBM API Connect';
apim.setvariable('message.body', json.body);

//add a new response header
json.headers.platform = 'Powered by IBM API Connect';
apim.setvariable('message.headers', json.headers);
```

If your consumer / backend is using XML, several policies are available for interacting with json and xml.
```
Map: provides schema to schema transformation
xslt: transforms xml documents using XSLT stylesheets
xml-to-json and json-to-xml: auto-generate XML and JSON payloads dynamically
Once the message is transformed, you could use a Validate action to verify the message.
```
Sample code of an operation calls out to the Google Geocode API to obtain location information about the provided zip code, then utilize a simple gatewayscript to modify the response and provide a formatted Google Maps link.  
Set response object variable to google_geocode_response.  
```
// Save the Google Geocode response body to variable
var mapsApiRsp = apim.getvariable('google_geocode_response.body');

// Get location attributes from geocode response body 
var location = mapsApiRsp.results[0].geometry.location;

// Set up the response data object, concat the latitude and longitude  
var rspObj = {
  "google_maps_link": "https://www.google.com/maps?q=" + location.lat + "," + location.lng  
};

// Save the output  	
apim.setvariable('message.body', rspObj);
```
