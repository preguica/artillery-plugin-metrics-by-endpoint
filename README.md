
# Extended Purpose

Allows applications to group endpoints using a given function.
The name of the function should be given in variable metricsProcessEndpoint and should be a global function.

# Purpose

Use this plugin to get a per-endpoint breakdown of latency and response codes in your Artillery HTTP tests.

# Usage

Install the plugin globally or locally, depending on your setup

```
// global plugin installation
npm install -g https://github.com/preguica/artillery-plugin-metrics-by-endpoint.git
```
 
Enable the plugin in the config
 
```
config:
  plugins:
    metrics-by-endpoint: {}
  variables:
    metricsProcessEndpoint : "myProcessEndpoint"
```

Function *myProcessEndpoint* should be defined as a global function in the processor code.
Example function:

```
// All requests for endpoints matching the following pattern and method will be aggregated under the same endpoint for the statistics
var statsRegExpr = [ [/.*\/rest\/media\/.*/,"GET","/rest/media/*"],
			[/.*\/rest\/media/,"POST","/rest/media"],
			[/.*\/rest\/user\/.*\/channels/,"GET","/rest/user/*/channels"],
			[/.*\/rest\/user\/.*/,"GET","/rest/user/*"],
			[/.*\/rest\/user\/.*\/subscribe\/.*/,"POST","/rest/user/*/subscribe/*"],
			[/.*\/rest\/user\/auth/,"POST","/rest/user/auth"],
			[/.*\/rest\/user/,"POST","/rest/user"],
			[/.*\/rest\/channel\/.*\/add\/.*/,"POST","/rest/channel/*/add/*"],
			[/.*\/rest\/channel/,"POST","/rest/channel"],
			[/.*\/rest\/channel\/.*\/messages.*/,"GET","/rest/channel/*/messages"],
			[/.*\/rest\/channel\/.*/,"GET","/rest/channel/*"],
			[/.*\/rest\/messages/,"POST","/rest/messages"],
			[/.*\/rest\/messages\/.*/,"GET","/rest/messages/*"]
	]

// Function used to compress statistics
global.myProcessEndpoint = function( str, method) {
	var i = 0;
	for( i = 0; i < statsRegExpr.length; i++) {
		if( method == statsRegExpr[i][1] && str.match(statsRegExpr[i][0]) != null)
			return method + ":" + statsRegExpr[i][2];
	}
	return method + ":" + str;
}
```



# License

MPL 2.0
