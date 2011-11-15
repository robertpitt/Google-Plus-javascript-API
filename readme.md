##About
This library is a javascript library that allows you to access the Google+ API from client side (Websites), it is built to be very lightweight and
to keep itself in-sync with google's changes

##Prerequisites
In order for you to use the client library you must first have an API key in order to access the protected resources, follow these instructions in order to get started

- Create your Application at the [Google Console](https://code.google.com/apis/console/)
- Enable Google+ by clicking the **API Access** in the menu
- Once enable you will have a dhashboard for your application, this is where you will find your **Simple API Access API Key**
- Download a copy of the GooglePlusAPI.js file and add it to your webserver

In your html, within the head of your document, you can load the javascript API like so

`<script type="text/javascript" src="/path/to/googleplusapi.compressed.js"></script>`

once this has been loaded you can then proceed to using the api.

##Creating an API object
After you have laoded the library you have to instantiate an object, you can accomplish that like so:

```
<script type="text/javascript">
    var GoogleAPI = new GooglePlusAPI('<Your-API-Key-Here>');
</script>
```

once this is established you can then use the `GoogleAPI` variable to access any ofthe following methods:

- `getPerson(id, options, callback)`
- `listByActivity(activityId, collection, options, callback)`
- `listActivities(id, options, callback)`
- `getActivity(id, options, callback)`
- `listComments(id, options, callback)`
- `getComment(id, options, callback)`
- `searchPeople(query, options, callback)`

you will notice a few things about these methods, the first thing is that there is always an `options` and a `callback` at the end of
each function, the options is a literal-object, the options parameter are option params for the request, see there documentation for keys and values.

if you do not wish to pass in any option params (`options`), then you simple fill it's space with an empty object `{}`, this param should always exists when calling a method

##The Callback
The callback's are always the same, there are 2 arguments passed into the callback when fired, the first is the `error` object, and second is the `result` object, if the request was successfull, the error object will be `null`, and the `result` object will be filled.

The error object and the result objects are relavent to the request your making, so if your calling `getPerson`, the result object will be a [Person Resource](https://developers.google.com/+/api/latest/people#resource)

##Getting the data

In order to use the api, you must first instantiate the object into a variable, see above, and then you can call the API like so:

```
GooglePlusAPI.getPerson('110106586947414476573', {}, function(error, response){

});
```

####Note:
The User ID number should **always** be treated as a string, and never and interger, this is due to how large the number is and have javascript modifies those large numbers.

within the callback we should always check for the error object, to assure that we have made a successfull request, this can be done like so:

```
GooglePlusAPI.getPerson('110106586947414476573', {}, function(error, result){
    if(error)
    {
        console.log('Error ' + error.code + ': ' + error.message);
        return; //!Important
    }
    
    //Here we have the result object, which is of kind person
});
```

##Methods and Callbacks

- `getPerson(id, options, callback)`
 - id: <String> UserID
- `listByActivity(activityId, collection, options, callback)`
 - id: <String> ActivityID
 - collection: <String> [plusones | reshares]
- `listActivities(id, options, callback)`
 - id: <String> ActivityID
- `getActivity(id, options, callback)`
 - id: <String> ActivityID
- `listComments(id, options, callback)`
 - id: <String> ActivityID
- `getComment(id, options, callback)`
 - id: <String> CommentID
- `searchPeople(query, options, callback)`
 - query: <String> Search Query

###The Request Method

The `request` method should not be used that often, but this allows you to make direct requests without using the methods
the method runs under the same pricipals as the others apart from teh first param is a path variable, this path variable is defined after the **/v1/** of the API base URL, so of I wanted to request the following URL

`https://www.googleapis.com/plus/v1/people/110106586947414476573`

i would call the following:

```
GooglePlusAPI.request('people/110106586947414476573', {}, function(error, result){
});
```

###using the option params argument

When using the optional params in the method calls, you do not have to any form of encoding as this is done for you, so if i wanted to search for people named **Robert Pitt** as well as set the `maxResults` to 5, I can call the following:

```
GooglePlusAPI.searchPeople('Robert Pitt', {maxResults: 5}, function(error, result){
    if(error)
    {
        console.log('Error ' + error.code + ': ' + error.message);
        return; //!Important
    }
    
    //result.items.length should be limited to 5
});
```

#Contact

If you have any questions then feel free to contact me on my Google Plus Page ([Robert Pitt](https://plus.google.com/110106586947414476573/))