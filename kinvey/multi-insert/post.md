# New Kinvey Feature Improves Access to Legacy Data Sets from Cloud Apps

Since the early days of SQL (that's about 40 years ago, but who's counting?) we've struggled to find an efficient and reliable way to insert multiple records into a database table in one stroke. Something that doesn't require an inordinate amount of code, confusing new proprietary syntax, or looping over arrays and data sets.

When the world started moving towards [NoSQL](https://en.wikipedia.org/wiki/NoSQL) data stores (like [Progress Kinvey](https://www.progress.com/kinvey)), those same fundamental issues still existed in many ways. Today we will cover Kinvey's solution for this problem: the multi-insert API.

## Small, Large, but Nothing in Medium! 👕

Since its inception, Kinvey has supported a standard REST API for data collections. Insert a record with a `POST`, select one or more records with a `GET`, replace a record with a `PUT`, or delete an entity with a `DELETE`. These are table stakes capabilities for any cloud or on-premise data store.

Kinvey added on to these capabilities by allowing users to support more complex data operations like multiple inserts, multiple updates, and partial updates by leveraging [Business Logic](https://devcenter.kinvey.com/rest/guides/business-logic).

While Business Logic and the [FlexServices API](https://devcenter.kinvey.com/rest/guides/flex-services) are powerful means of crafting server-side logic for your apps, sometimes the overhead required can be overkill. Especially for something as "simple" as just trying to insert records into a data collection. So customers have been clamoring for a way to add some kind of "bulk insert" feature into the Kinvey REST API (or one of the [many other client SDKs](https://devcenter.kinvey.com/)). The answer is the new multi-insert API.

## Multi-Insert in Kinvey 📩

Instead of writing a `for` loop and iterating over an array of values, Kinvey now allows you to send a batch of entities to be inserted, all at once. And it should be noted that multi-insert is supported in the Kinvey SDKs as of v5.

> To make a v5 API request, be sure to set the API version in the request with the `X-Kinvey-API-Version` HTTP header.

These multi-record inserts are performed with a `POST` request and an array of entities:

	POST /appdata/:appKey/:collectionName HTTP/1.1
	Host: baas.kinvey.com
	Authorization: [YOUR-APP-CREDENTIALS]
	Content-Type: application/json
	X-Kinvey-API-Version: 5
	
	[
	  { "_id": 1, "firstname": "Rob", "lastname": "Lauer" },
	  { "_id": 2, "firstname": "Dan", "lastname": "Wilson" },
	  { "_id": 3, "firstname": "TJ", "lastname": "VanToll" },
	  { "_id": 4, "firstname": "Sebastian, "lastname": "Witalec" }
	]

In the response to this `POST`, you'll see an array of `entities` and an array of `errors`. The `entities` containing the records inserted (with a `null` as a placeholder for any issues) and the `errors` containing any errors logged (with the index position of the entity in the original array).

For example, in this request, if the record of "Sebastian" failed, your response would look like this:

	{
	  "entities": [
	    {
	      "_id": 1,
	      "firstname": "Rob",
		  "lastname": "Lauer"
	    },
	    {
	      "_id": 2,
	      "firstname": "Dan",
		  "lastname": "Wilson"
	    },
	    {
	      "_id": 3,
	      "firstname": "TJ",
		  "lastname": "VanToll"
	    },
	    null
	  ],
	  "errors": [
	    {
	      "code": "<err code>",
	      "index": 3,
	      "errmsg": "<err msg>"
	    }
	  ]
	}

When looking at how this is performed in some of the individual client SDKs, you'll find the syntax to be quite familiar. For example, with the JavaScript SDK:

	Kinvey.init({
	  appKey: '<app key>',
	  appSecret: '<app secret>',
	  apiVersion: 5
	});
	
	const newPeople = [
	  { firstname: 'Joe' },
	  { firstname: 'Jane' }
	];

	const store = Kinvey.DataStore.collection('People', Kinvey.DataStoreType.Network);
	
	await store.save(newBooks);

You will be able to find examples for the other client SDKs in the [Kinvey DevCenter](https://devcenter.kinvey.com/).

## Advantages of Multi-Insert 💯

Aside from improving the developer experience with a more logical means of bulk inserting records, you can also achieve vastly improved performance when connecting to external data sources with both [RapidData](https://devcenter.kinvey.com/rest/guides/rapid-data) and FlexServices.

FlexServices can handle a new multi-insert request by utilizing the `onInsertMany` event. As above, the request is an array of entities.  

	const sdk = require('flex-sdk');
	
	function somePeopleFunction(context, complete, modules) {
	  const body = context.request.body;  // <-- this is an array
	  // do some code magic!
	  const result = { entities: [], errors: [] };
	  complete().setBody(result).ok().done();
	}
	
	sdk.service((err, flex) => {
	  const data = flex.data;
	  data.collection('People').onInsertMany(somePeopleFunction);
	});


## Before You Get Started... 🏁

Please note that this feature is in EA (early access) mode while we work out the kinks. If you'd like to help us try it out, please reach out to our support team directly at support@kinvey.com. We estimate the feature will be generally available in Q2!

If you haven't already, sign up for your [free Progress Kinvey trial today!](https://console.kinvey.com/sign-up)