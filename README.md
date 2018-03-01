# json-xml-rpc
JavaScript JSON/XML-RPC Client Implementations

## Getting Started

Include <script type="text/javascript" src="rpc.js"></script> in your HTML body section.

## Usage examples
***
var service = new rpc.ServiceProxy("/app/service", {
                          asynchronous: true,   //default: true
                          sanitize: true,       //default: true
                          methods: ['greet'],   //default: null (synchronous introspection populates)
                          protocol: 'JSON-RPC', //default: JSON-RPC
  }); 
  service.greet({
     params:{name:"World"},
     onSuccess:function(message){
         alert(message);
     },
     onException:function(e){
         alert("Unable to greet because: " + e);
         return true;
     }
  });
 ***
  If you create the service proxy with asynchronous set to false you may execute
  the previous as follows:
 ***
  try {
     var message = service.greet("World");
     alert(message);
  }
  catch(e){
     alert("Unable to greet because: " + e);
  }
 ***
  Finally, if the URL provided is on a site that violates the same origin policy,
  then you may only create an asynchronous proxy, the resultant data may not be
  sanitized, and you must provide the methods yourself. In order to obtain the
  method response, the JSON-RPC server must be provided the name of a callback
  function which will be generated in the JavaScript (json-in-script) response. The HTTP GET
  parameter for passing the callback function is currently non-standardized and so
  varies from server to server. Create a service proxy with the option
  'callbackParamName' in order to specify the callback function name parameter;
  the default is 'JSON-response-callback', as used by associated JSON/XML-RPC
  Server project. For example, getting Google Calendar data:
 ***
  var gcalService = new rpc.ServiceProxy("http://www.google.com/calendar/feeds/myemail%40gmail.com/public", {
                          asynchronous: true,  //true (default) required, otherwise error raised
                          sanitize: false,     //explicit false required, otherwise error raised
                          methods: ['full']    //explicit list required, otherwise error raised
                          callbackParamName: 'callback'
                          }); 
  gcalService.full({
       params:{
           alt:'json-in-script' //required for this to work
           'start-min':new Date() //automatically converted to ISO8601
           //other Google Calendar parameters
       },
       onSuccess:function(json){
           json.feed.entry.each(function(entry){
               //do something
           });
       }
  });
  ***
## License

This project is licensed under the GPL/MIT License.
