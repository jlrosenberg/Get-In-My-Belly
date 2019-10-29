### GET API/v1/recipes
This is the endpoint that is used to fetch a list of recipes that meet a certain set of criteria. This parameters are passed in through the url. Below are a few examples of this format.
* GET api/v1/recipes?authorId={ID}
* GET api/v1/recipes?tagIds={ID}
* GET api/v1/recipes?authorId={ID}&tagIds={ID2, ID3}
* GET api/v1/recipes?favoritedBy={ID}
...

The three query parameter types initially supported will be authorId, favoritedBy, and tagIds. authorId and favoritedBy will only accept one ID in a query, whereas tagIds should allow you to provide as many as you want, seperated by commas.

The results provided by this endpoint make no garuntees about order. Each recipe will need to be fetched by the client, and the client can order and render recipes in any method they wish. In the future, this would be a good thing to change for performance benefits - the endpoint could take in a sortBy parameter and sort on the backend instead, but that kind of feature does not belong in a v1 endpoint.

A user does not need to be authenticated or signed in to hit this endpoint.
```json
{
  "status":"200",
  "path":"api/v1/recipes?authorId=3&tagIds=2,3,4,5,7,24",
  "method":"GET",
  "response":{
    "count":"3",
    "recipes":[
      "http://www.getinmybelly.com/api/v1/recipes/23", "http://www.getinmybelly.com/api/v1/recipes/27", "http://www.getinmybelly.com/api/v1/recipes/35"
    ]
  }
}
```

### POST api/v1/recipes
Create a new recipe. In order to use this endpoint, the request must be made by a signed in user. If the request is not coming from an authenticated user, it will return a 403 Forbidden Error. 

```json
{
  "path":"api/v1/recipes",
  "method":"POST",
  "recipe":{
    "name":"Strawberry Smoothie",
    "authorId":"5",
    "tagIds":[ "5", "6", "9"],
    "description":"This is a quick and easy breakfast smoothie",
    "bannerImgUrl":"http://www.link.to.banner.image.com",
    "images":["www.link1.com", "www.link2.com"],
    "ingredients":[
      { "ingredientId":"5", "quantity":"2", "measurementUnitId":"4" },
      { "ingredientId":"7", "quantity":"1.5", "measurementUnitId":"2" }
    ],
    "steps":[
      { "order": "1", "instruction": "First, you take the strawberries and you put them in the blender. Add mango chunks and yogurt"},
      { "order": "2", "instruction": "Next, put the blender on pulse, and blend until smoothie is smooth with no chunks. Serve immediately for best smoothie experience."},
    ]
  }
}
```
### GET api/v1/measurement_units
Returns a list of measurement units currently supported by the application. The user will need to choose between these units when adding an ingredient to a recipe. A user does not need to be logged in or authenticated to use this endpoint.
```json
{
  "measurementUnits":[
    { "id":"1", "singularName":"cup", "pluralName":"cups" },
    { "id":"2", "singularName":"tsp", "pluralName":"tsp" },
    { "id":"3", "singularName":"tbsp", "pluralName":"tbsp" },
    { "id":"4", "singularName":"pinch", "pluralName":"pinches" },
    { "id":"5", "singularName":"gram", "pluralName":"grams" },
    { "id":"6", "singularName":"oz", "pluralName":"ounces" }
  ]
}
```

### POST api/v1/ingredients
Create a new ingredient. This will 403 if the user is not logged in, and will 500 if an ingredient exists with the same name already.
```json
{
  "path":"api/v1/ingredients",
  "method":"POST",
  "ingredient":{
    "name":"Flour",
    "defaultMeasurementUnitsId":"1"
  }
}
```

On success, you should recieve the following response:
```json
{
  "status":"200",
  "ingredient":{
    "id": "2",
    "name":"Flour",
    "defaultMeasurementUnitsId":"1"
  }
}
```

### GET api/v1/ingredients
Returns all of the ingredients currently in the database.

```json
{
  "status":"200",
  "path":"api/v1/ingredients",
  "method":"GET",
  "response":{
    "ingredients":[
      {
        "id": "5",
        "name": "Almond Flour",
        "defaultMeasurementUnitsId": "2"
      },
      {
        "id": "4",
        "name": "Eggs",
        "defaultMeasurementUnitsId": "1"
      },
      {
        "id": "7",
        "name": "Baking Powder",
        "defaultMeasurementUnitsId": "3"
      },
      {
        "id": "24",
        "name": "Butter",
        "defaultMeasurementUnitsId": "5"
      }
    ]
  }
}
```

### PATCH api/v1/recipes{id}
TODO - need to decide if we want to do a PUT or a PATCH for modifying. Also need to figure out how to handle when the user wants to change order of steps, this is actually tricky I think.

### GET api/v1/recipes/{id}
Returns detailed information about a recipe. This is intended to be used to render the full recipe. A user does not need to be authenticated or logged in to hit this endpoint
```json
{
  "status":"200",
  "path":"api/v1/recipes/18",
  "method":"GET",
  "response":{
    "id":"18",
    "title":"90 Second Almond Bread",
    "authorId":"7",
    "authorName":"Mr. Wonderful",
    "description":"This bread is great for a snack, breakfast, or to go along with a meal or even along with a coffee. It's easy to make, and only takes ~5 minutes from start until it is ready to eat!",
    "bannerImageUrl":"https://www.bannerimage.link.here",
    "images":[
      "www.link1.com/img.jpg", "www.link2.com/img.jpg", "www.link3.com/img.jpg"
    ],
    "ingredients":[
      {
        "id": "5",
        "name": "Almond Flour",
        "quantity": "1/3",
        "units": "cups"
      },
      {
        "id": "4",
        "name": "Eggs",
        "quantity": "1",
        "units": "unit"
      },
      {
        "id": "7",
        "name": "Baking Powder",
        "quantity": "1/2",
        "units": "tsp"
      },
      {
        "id": "24",
        "name": "Butter",
        "quantity": "1",
        "units": "tbsp"
      }
    ],
    "steps":[
      {
        "id": "13",
        "order": "1",
        "instruction": "First, put the butter in the mug. Put it in the microwave and heat at full power for 45 seconds."
      },
      {
        "id": "14",
        "order": "2",
        "instruction": "Next, combine all of the dry ingredients in the mug. Then, add the egg in, and stir thoroughly until mixed. (OPTIONAL) You can also throw in a pinch of salt."
      },
      {
        "id": "15",
        "order": "3",
        "instruction": "Put the mug back in the microwave, and heat at full power for 90 seconds."
      }
    ]
  }
}
```


### GET api/v1/tags
Returns a list of existing recipe tags, along with the count of how many recipes exist with each of these tags. A user does not need to be authenticated or signed in to hit this endpoint.
```json
{
  "status":"200",
  "response":{
    "tags":[
      {
        "tagId": "34",
        "tagName":"Summer Smoothies",
        "count": "47"
      },
      {
        "tagId": "79",
        "tagName":"Desserts",
        "count": "745"
      },
      {
        "tagId": "53",
        "tagName":"Quick Snacks",
        "count": "353"
      }
    ]
  }
}
```


### GET api/v1/user/{id}

Get the information about a user. This includes the basic information (i.e name, profile picture), along with the the 3 recipes that the user has most recently added to their favorites, and the 3 recipes written by the user that have recieved the most favorites. If the user does not have 3 recipes that fall into either of these bins, then as many as possible are returned. If no recipes exist that fall into these bins (i.e. the user has no favorites, or hasn't submitted any recipes) then an empty array is return for these attributes. This endpoint does not require a user to be logged in or authorized.

This endpoint is designed to be used to render a public profile for a user, not to contain any user settings, configurations, or confidential information.

```json
{
  "status": "200",
  "response":{
    "id": "2",
    "firstName": "Mark",
    "lastName": "Cuban",
    "profilePictureUrl": "http://www.cloud.bucket.link.to.picture",
    "dateJoined": "10/03/2018",
  }
}
```



### POST api/v1/user

Create a new account. This will be rejected with a 500 error if a user with this email already exists.
THIS ENDPOINT IS NOT YET FINISHED - I NEED TO FINISH FIGURING OUT HOW WE ARE DOING USER AUTHENTICATION
```json
{
  "user":{
    "firstName": "Mark",
    "lastName": "Cuban",
    "email": "markcuban@sharks.com",
    "profilePictureUrl": "http://www.link.to.profile.pic.com/img.jpg"
    // I'm not sure how it works creating a new account to set the password, obviously we can't transmit it as raw text. I will figure that portion out when I have internet to google how its usually done with devise for small Rails projects.
  }
}
```

On success, you should recieve the following response:
```json
{
  "status":"200",
  "user":{
    "id": "2"
  }
}
```

### POST api/v1/login
```json
{
  "TODO":"TODO"
}
```

On success, you should recieve the following response:
```json
{
  "TODO":"TODO"
}
