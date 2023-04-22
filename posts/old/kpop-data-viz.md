# Kpop Data Visualizations

This blog post contains information regarding certain parts of the project. It layouts how the technologies I used work, their purposes in the project, etc. Learning from my past experiences, documentation is very important because you bet I won't remember any of this once the project becomes inactive.

---

## Using HTTPS Request with Node.js Standard Library

### Purpose
I need to make requests to the Spotify Web API to get the data. There was a `request` module in Javascript, much like Python's `requests` library, but it became decaprecated. So instead I am using the `https` module in the Node.js Standard Library. So no third-party packages.

### How it works
The `https` module provides two methods:
1. `https.request(url,[options],[callback])` or `https.request([options][callback])`
2. `https.get(url,[options],[callback])`
*https.get is a convience method that functions the same as https.request*

The official documentation is [here]((https://nodejs.org/docs/latest-v17.x/api/http.html#httprequesturl-options-callback)). Here's [another documentation link](https://nodejs.org/docs/latest-v17.x/api/http.html#class-httpclientrequest) that explains the callback function a bit more.

The provided examples are relatively easy to read, but here is how I am using it (explanations are in the comments):
```javascript
// I need to encode this since I am sending application/x-www-form-urlencoded data
const form = encodeURI('grant_type') + '=' + encodeURI('client_credentials');

// In options, I specify the parameters of my request
// I include the URI information, what method I'm using, authorization string, and headers properties
const options = {
    host: 'accounts.spotify.com',
    path: '/api/token',
    method: 'POST',
    auth: CLIENT_ID + ':' + CLIENT_SECRET,
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
    }
};

// Here I am building the request
// I pass in the options (the url is optional and the callback function for when the response is returned
const req = https.request(options, (res) => {

    console.log(`STATUS: ${res.statusCode}`);

    res.setEncoding('utf8');

    let rawData = '';

    // data listener/handler to consume the data from response
    res.on('data', (chunk) => { rawData += chunk;});

    // end listener on what to do when there is no more data to consume
    res.on('end', () => {
        try{
            // convert data to JSON object
            console.log(JSON.parse(rawData));
        }catch(e){
            console.error(e.message);
        }
    });
});

// write() will write the form data to the request body
req.write(form);
// end() sends off the request kind of
req.end();
```
*Notes:*  
- *Knowing exactly what [`req.end()`](https://nodejs.org/docs/latest-v17.x/api/http.html#requestenddata-encoding-callback) does isn't too important. Might want to look into that later though*
- *https `request()` and `get()` work the same way as the http module*
- *application/x-www-form-urlencoded needs [special encoding](https://stackoverflow.com/questions/35473265/how-to-post-data-in-node-js-with-content-type-application-x-www-form-urlencode)*
- *`auth` property in `options` already encodes it in base64, so there is no need for a [`Buffer` object](https://github.com/spotify/web-api-auth-examples/blob/master/client_credentials/app.js) as shown the Spotify example*

---

## Using the Spotify API

### How it works
I am using the [*Client Credentials Authorization flow*](https://developer.spotify.com/documentation/general/guides/authorization/client-credentials/) This flow doesn't require me to ask for user permissions (since I won't be using any user data), so I get to bypass the whole redirect thing. 

Here are the steps:
1. I [create](https://developer.spotify.com/documentation/general/guides/authorization/app-settings/) a Spotify app.
2. I set the *client id* and *client secret* as environment variables in my app.
3. I make a POST request to *accounts.spotify.com/v1/token*, with the client id and secret as part of the Authorization string in the header (and other parameters listed in the documentation), to obtain an *access token*.
4. I make a GET request to any of the Spotify API endpoints (e.g. *api.spotify.com/v1/artists/<id>*) with the access token as part of the Authorization string.
5. I have data.

### Pagination

For many of the Spotify API endpoints, they separate the data into different "pages" and have an optional `offset` parameter for the request. They also include a `next` parameter which includes a link to the next set of data.

There were a couple solutions, but I decided on using the `next` parameter. The `next` parameter is either a link or `null` (if there are no more pages), so I can use a while loop that keeps requesting the `next` url right after the initial request to the endpoint. So in the code, I have:
- a generic GET request promise that sends the access token Authorization string (helper function `getRequest()` in `/server/data.js`)
- a function `getData()` in `/server/data.js` that determines what URL to send and (uses the helper function to send a request)

Here is what the while loop looks like:
```javascript
    try{
        // get the first page
        let data = await getRequest(accessToken, endpointUrl);
        console.log(data);
        
        // for further pages, request with provided next url
        while(data.next){
            data = await getRequest(accessToken, data.next);
            console.log(data);
        }

    }catch(error){
        console.error(error.message);
    }
```



---

## Using Promises, `async/await`

### Purpose
To work with the asynchronous API calls in the program; I need to ensure that data is returned before moving on the another part of the program like in:
```javascript
var accessToken = auth.getAccessToken();
console.log('Access Token: ' + accessToken);
```

### How it works

DigitalOcean made a [fantastic article](https://www.digitalocean.com/community/tutorials/understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript#using-the-fetch-api-with-promises) about asynchronous Javascript. Essentially, I need to use promises and `async/await` to ensure I get data returned before anything else happens.


## Using MongoDB and Mongoose


## Exporting functions between Files

Here is how to export functions between modules: [NodeJS guide](https://nodejs.org/en/knowledge/getting-started/what-is-require/) and [tutorial](https://www.stanleyulili.com/node/node-modules-import-and-use-functions-from-another-file/#:~:text=To%20include%20functions%20defined%20in%20another%20file%20in%20Node.,functions%20using%20the%20dot%20notation.)