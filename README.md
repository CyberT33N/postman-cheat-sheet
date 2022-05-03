# Postman Cheat Sheet
Postman Cheat Sheet with the most needed stuff..



<br>
<br>

## Pre-request Script

#### Get Bearer Token and set if before request
```javascript
var server          = pm.variables.get("server"); // add your Keycloak-URL here (without /auth)
var realm           = pm.variables.get("realm"); // the name of the realm
var grantType       = "password"; // the granttype, with password you can login as a normal user
var clientId        = pm.variables.get("clientId"); // the name of the client you created in Keycloak
var clientSecret    = pm.variables.get("clientSecret"); // the secret you copied earlier
var username        = pm.variables.get("username"); // the username of the user you want to test with
var password        = pm.variables.get("password"); // the password of the user you want to test with

// creating the request URL
var url  = `${server}/auth/realms/${realm}/protocol/openid-connect/token`;
// creating the body of the request
var data = `grant_type=${grantType}&client_id=${clientId}&username=${username}&password=${password}&client_secret=${clientSecret}`;

console.log('url:', url)
console.log('data:', data)

// request to Keycloak
// read more about this here: https://www.keycloak.org/docs/latest/authorization_services/#_service_overview
pm.sendRequest({
    url: url,
    method: 'POST',
    header: { 'Content-Type': 'application/x-www-form-urlencoded'},
    body: {
        mode: 'raw',
        raw: data
    }
},  function(err, response) {
    // Set the environment variable for token
    if(err){
        console.log('an error occured')
        console.log(JSON.stringify(err))
    }else{
        var response_json = response.json();
        console.log('response_json:', response_json);

        var token = response_json.access_token;
        pm.environment.set('access_token', token);
        // You can open up the console in Postman with Alt + Ctrl + C
        console.log('token:', token);
pm.environment.unset("variable_key");
    }
});
```

- Dann muss der Auth Type auf Bearer Token im jeweiligen Request gesetzt werden. Authorization → Bearer Token. Bei Token dann als Value eintragen:
```
{{access_token}}
```
