#client-templates    
Boilerplate for LIVE websites

###SSE, and client side rendering.  Pure HTML, JS only.

####Server
````
var express = require('express');
var app = express();

var sse = require('./sse.js');
app.all('/feed', sse.handle);

app.listen(80);
console.log('started 80');

require('./trends.js')(function(s){
  sse.send(s);
});
````

####Client
````
<!DOCTYPE html>
<link href='css.css' rel='stylesheet'>

<body class=roboto>

<center>
<h1>Five Minute Trends</h1>

<div id='template'>
 <div class='three left padding'>
 {{#users}}
  <a target=_ href=http://twitter.com/{{name}}>{{name}}</a> <small title='score'>{{score}}</small><br>
 {{/users}}
 </div>

 <div class='three left padding' dir="">
 {{#tags}}
  <a target=_ href=http://twitter.com/{{name}}>{{name}}</a> <small title='score'>{{score}}</small><br>
 {{/tags}}
 </div>
 </center>
</div>

</body>

<script>
  function catchEvents(data){
    data.users = data.users.map(function(u){return {name:u[0], score:u[1]}});
    data.tags = data.tags.map(function(u){return {name:u[0], score:u[1]}});
  	return data;
  }
</script>

<script x-updates='/feed' x-process='catchEvents' src='render.js'></script>
````