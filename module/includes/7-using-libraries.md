# Using Libraries

It's useful to test an API by sending queries over a browser's URL, complete with a query string populated by an access token, but in a production app, you might need to make a more complicated API call with various data assembled together to form a query.

Many developers, therefore, rely on libraries that standardize the process of working with APIs. For JavaScript developers, Axios is an excellent choice. Python programmers might use Request. Using Powershell? Try RestMethod.

Imagine that you are passionate about clock radios, and are interested in the interesting collection of well-designed examples at the Cooper Hewitt. You would construct a query with your API access token with this format, given that you want paginated results and only results with images: 

`https://api.collection.cooperhewitt.org/rest/?method=cooperhewitt.search.collection&access_token=<your access token>&query=clock%20radio&has_images=true&page=1&per_page=100`, as specified in the museum's API documentation.

The first example that comes up is this slick example from the 1950s:

![1957 clock radio](https://images.collection.cooperhewitt.org/39369_1f0c462c864fbf4d_b.jpg)

> To test API endpoints, [Postman](https://www.postman.com/) is an excellent tool.

::: zone pivot="javascript"

var axios = require('axios');

var config = {
  method: 'get',
  url: 'https://api.collection.cooperhewitt.org/rest/?method=cooperhewitt.search.objects&query=clock%20radio&page=1&per_page=100&access_token=yourtoken',
  headers: { }
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});

::: zone-end

::: zone pivot="python"

import requests

url = "https://api.collection.cooperhewitt.org/rest/?method=cooperhewitt.search.objects&query=clock%20radio&page=1&per_page=100&access_token=yourtoken"

payload = {}
headers= {}

response = requests.request("GET", url, headers=headers, data = payload)

print(response.text.encode('utf8'))

::: zone-end

::: zone-pivot="powershell"

$response = Invoke-RestMethod 'https://api.collection.cooperhewitt.org/rest/?method=cooperhewitt.search.objects&query=clock%20radio&page=1&per_page=100&access_token=yourtoken' -Method 'GET' -Headers $headers -Body $body
$response | ConvertTo-Json

::: zone-end

By using libraries appropriate to your preferred programming language, you can have more control over how you leverage APIs from within your code as you script functions based on user interactions such as a button click.

## Write to an endpoint with a method

Many third party endpoints like these museum examples do not allow writing (using 'POST') to their databases, as their purpose is not transactional. However you can practice using these methods on your own data. 

If you return to the `db.json` file you created earlier in this module, you can write data to your personal database.

Using your terminal, `cd` to the location of your `db.json` file and type `json-server --watch db.json`. The server will start on port 3000 and you can query it using Postman by using `GET` for your objects collection: `localhost:3000/objects`.

To write to this collection by setting a header value: `'content-type': 'application/json'` and passing through a JSON object, stringified:

::: zone pivot="javascript"

var axios = require('axios');
var data = JSON.stringify({"id":5,"item":"The Fiancés","artist":"Pierre Auguste Renoir","collection":"Wallraf–Richartz Museum, Cologne, Germany","date":"1868"});

var config = {
  method: 'post',
  url: 'localhost:3000/objects',
  headers: { 
    'content-type': 'application/json'
  },
  data : data
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});

::: zone-end

::: zone pivot="python"

import requests

url = "localhost:3000/objects"

payload = "{\n        \"id\": 5,\n        \"item\": \"The Fiancés\",\n        \"artist\": \"Pierre Auguste Renoir\",\n        \"collection\": \"Wallraf–Richartz Museum, Cologne, Germany\",\n        \"date\": \"1868\"\n    }"
headers = {
  'content-type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))

::: zone-end

::: zone-pivot="powershell"

$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("content-type", "application/json")

$body = "{`n        `"id`": 5,`n        `"item`": `"The Fiancés`",`n        `"artist`": `"Pierre Auguste Renoir`",`n        `"collection`": `"Wallraf–Richartz Museum, Cologne, Germany`",`n        `"date`": `"1868`"`n    }"

$response = Invoke-RestMethod 'localhost:3000/objects' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json

::: zone-end

You can use your personal database and API endpoint to try other operations, using your library of choice to `GET`, `POST`, and even `DELETE` items from your collection.