# Going Off the Grid with NativeScript

While access to the internet has never been more widely available, we still live in an occasionally disconnected world. Whether on an airplane, underground, or in a rural area on 2G, we routinely deal with varied speeds and spotty access. Yet we expect our mobile apps to continue to work flawlessly!

> Approximately 60% of all mobile users are limited to 2G networks.

When building a mobile app, it's up to us as app developers to make sure the offline scenario is addressed. Let's look at how, when building a [NativeScript](https://www.nativescript.org/) app, we make sure our app can deal with little to no network access.

- [mBaaS and Offline Data Sync](#mbaas)
- [Offline Data Storage](#offline-data)
- [Offline App Resources](#offline-resources)
- [Testing Offline Scenarios](#offline-testing)

<a name="mbaas"></a>
## mBaaS and Offline Data Sync

Utilizing a "Mobile Backend as a Service" (mBaaS) is the gold standard of mobile data storage and retrieval. Cloud services like [Google Firebase](https://firebase.google.com/) and [Telerik Backend Services](http://www.telerik.com/platform/backend-services) make our lives easier by providing an easy to use API to store and retrieve data.

But if your data lives in the cloud, what happens when the cloud is not there? Thankfully, mBaaS providers usually provide an offline API. Let's take a closer look at the Firebase and Telerik Backend Services implementations:

### Google Firebase

![google firebase logo](firebase.png)

Your first step with Firebase is to leverage the [NativeScript plugin for Firebase](https://github.com/EddyVerbruggen/nativescript-plugin-firebase). The docs provided are extensive, but there are two critical pieces of functionality you will probably want to utilize for offline scenarios:

**1) Enable disk persistence.** When initializing your Firebase db, you can pass `persist: true` to the `init` function and Firebase will save data to the device by default, making it available when offline:

	firebase.init({
		persist: true
	});

**2) Sync your data.** While Firebase automatically syncs and stores local data for active listeners, you can also keep specific endpoints in sync regardless of the state of the app:

	firebase.keepInSync(
		"/users", // which path in your Firebase needs to be kept in sync?
		true      // set to false to disable this feature again
	).then(
		function () {
			console.log("firebase.keepInSync is ON for /users");
		},
		function (error) {
			console.log("firebase.keepInSync error: " + error);
		}
	);

FYI, if your cache grows larger than its pre-configured size (10MB), Firebase will purge the oldest data first.

### Telerik Backend Services

![telerik backend services logo](telerik-backend-services.png)

Offline support in Telerik Backend Services is accomplished by simply setting the `offline` property to true:

	var el = new Everlive({
		appId: 'your-app-id',
		offline: true
	});

*Note that `Everlive` in code is the legacy name of Telerik Backend Services.*

There are other properties to consider too, such as the [location of saved data](http://docs.telerik.com/platform/backend-services/javascript/offline-support/offline-storage-provider) and the [encryption mechanism](http://docs.telerik.com/platform/backend-services/javascript/offline-support/offline-data-security):

	var el = new Everlive({
		appId: 'your-app-id',
		offline: {
			storage: {
				provider: Everlive.Constants.StorageProvider.FileSystem
			},
			encryption: {
				provider: Everlive.Constants.EncryptionProvider.Default,
				key: 'your-encryption_key_here'
			}
		}
	});

Ideally you would generate your encryption key and store it in a secure location using the [NativeScript Secure Storage plugin](https://github.com/EddyVerbruggen/nativescript-secure-storage).

What about data sync with Telerik Backend Services? [The SDK](http://docs.telerik.com/platform/backend-services/javascript/getting-started-javascript-sdk) constantly registers the state of each offline item and knows if the item has been modified, created, or deleted. When your app comes back online, the SDK will automatically synchronize changes.

<a name="#offline-data"></a>
## Offline Data Storage

Maybe storing your data in the cloud just doesn't feel right. Maybe you are worried about security or managing potential data conflicts - or you just don't need it! Luckily you can always fall back on the tried and true method of managing an embedded database.

On-device data storage has obvious benefits in offline scenarios. No network access is required to store or retrieve data. You can also safely encrypt data at rest.

What are some of the best ways to store data locally with a NativeScript app? Let's take a look at some options:

### SQLite

![sqlite logo](sqlite.png)

[SQLite](https://sqlite.org/) is the most popular embedded relational database available today. It runs on virtually every platform out there and requires very little configuration to get up and running. And you use standard SQL to query your data.

To get started with SQLite on NativeScript, you'll want to install the [NativeScript SQLite plugin](https://www.npmjs.com/package/nativescript-sqlite):

	tns plugin add nativescript-sqlite

You can then create a new SQLite database (if it doesn't already exist) and execute a variety of queries:

	// require the sqlite module
	var sqlite = require("nativescript-sqlite");
	
	// create a table
	var createTable = function() {
	    new sqlite("test.db", function(err, db) {
	        db.execSQL("CREATE TABLE IF NOT EXISTS TestTable (id INTEGER PRIMARY KEY ASC, first_name TEXT, last_name TEXT)", [], function(err) {
	            console.log("TABLE CREATED");
	        });
	    });
	}
	
	// delete a table
	var deleteTable = function() {
	    new sqlite("test.db", function(err, db) {
	        db.execSQL("DROP TABLE IF EXISTS TestTable", [], function(err) {
	            console.log("TABLE DROPPED");
	        });
	    });
	}
	
	// insert a new record
	var insertRecord = function(first, last) {
	    new sqlite("test.db", function(err, db) {
	        db.execSQL("INSERT INTO TestTable (first_name, last_name) VALUES (?,?)", [first, last], function(err, id) {
	            console.log("The new record id is: " + id);
	        });
	    });
	}
	
	// update an existing record
	var updateRecord = function(id, first, last) {
	    new sqlite("test.db", function(err, db) {
	        db.execSQL("UPDATE TestTable SET first_name = ?, last_name = ? WHERE id = ?", [first, last, id], function(err, id) {
	            console.log("The existing record id is: " + id);
	        });
	    });
	}
	
	// delete a record
	var deleteRecord = function(id) {
	    new sqlite("test.db", function(err, db) {
	        db.execSQL("DELETE FROM TestTable WHERE id = ?", [id], function(err, id) {
	            console.log("The deleted record id is: " + id);
	        });
	    });
	}
	
	// select a single record
	var selectRecord = function(id) {
		new sqlite("test.db", function(err, db) {
			db.get("SELECT * FROM TestTable WHERE id = ?", [id], function(err, row) {
				console.log("Row of data was: " + row);  // Prints [["Field1", "Field2",...]] 
			});
		});
	}
	
	// select all records
	var selectAllRecords = function() {
		new sqlite("test.db", function(err, db) {
			db.all("SELECT * FROM TestTable ORDER BY id", [], function(err, rs) {
				console.log("Result set is: " + rs); // Prints [["Row_1 Field_1" "Row_1 Field_2",...], ["Row 2"...], ...]
			});
		});
	}

For more information on using SQLite with NativeScript, [check out this blog post](https://www.thepolyglotdeveloper.com/2016/04/use-sqlite-save-data-telerik-nativescript-app/) from Nic Raboy.

### Couchbase

![couchbase logo](couchbase.png)

If utilizing a [NoSQL](https://en.wikipedia.org/wiki/NoSQL) database is more of your thing, you should take a look at the [Couchbase Lite plugin for NativeScript](https://github.com/couchbaselabs/nativescript-couchbase). To get started with [Couchbase](http://www.couchbase.com/), simply run the plugin install command:

	tns plugin add nativescript-couchbase

And then create/open a database to manage your document objects:

	// require the couchbase module
	var couchbase = require("nativescript-couchbase");
	
	// create or open a database
	var db = new couchbase.Couchbase("testdb");
	
	// create a document
	var doc = db.createDocument({
		"first": "Rob",
		"last": "Lauer",
		"company": "Progress"
	});
	
	// retrieve a document
	var person = db.getDocument(doc);
	
	// update a document
	db.updateDocument(doc, {
		"first": "Rob",
		"last": "Lauer",
		"company": "Progress"
	});
	
	// delete a document
	db.deleteDocument(doc);

### Local Storage

Wait, "local storage"? Is this a web app? Of course not, but a similar concept exists for NativeScript! If you've ever used the local storage API before, you'll be quite comfortable with leveraging NativeScript's [application settings](http://docs.nativescript.org/api-reference/modules/_application_settings_.html):

	var appSettings = require("application-settings");
	
	// numbers (get/set)
	appSettings.setNumber("someNumber", 7);
	var someNumber = appSettings.getNumber("someNumber");

	// booleans (get/set)
	appSettings.setBoolean("someBool", true);
	var someBool = appSettings.getBoolean("someBool");

	// strings (get/set)
	appSettings.setString("someString", true);
	var someString = appSettings.getString("someString");

	// remove one data element
	appSettings.remove("someString");

	// remove everything
	appSettings.clear();

> Note that data stored in application settings does not persist after an app has been completely closed!

### File System

Leveraging the native device file system is another option that *does* persist data after app restarts. While not quite as simple as the get/set syntax of application settings, the file system can be a quick way to store data. For example:

	var fs = require("file-system");
	var fileName = "myfile.json";
	
	var file = fs.knownFolders.documents().getFile(fileName);
	var data = [{"id": "1", "value": "NativeScript"}, {"id": "2", "value": "Rocks"}]; 
	
	// save the text to the file system
	file.writeText(JSON.stringify(data));
	
	// read the text from the file
	file.readText().then(function(content) {
	  // content is the variable containing our data
	});
	
	// clear the file
	file.clear();

<a name="#offline-resources"></a>
## Offline App Resources

If you're trying to keep your app size down by downloading images and videos on-demand, you may want to re-think that strategy. NativeScript provides an `App_Resources` directory which allows you to leverage the native storage mechanisms of iOS and Android.

For example, to [display an image](https://docs.nativescript.org/ui/images) in your app, you can use markup like this:

	<Image src="res://logo" />

The `res://` prefix tells NativeScript to retrieve that resource from `App_Resources`. In fact, NativeScript leverages native methods for loading the best image for the current display density.

<a name="#offline-testing"></a>
## Testing Offline Scenarios

![airplane mode](airplane-mode.png)

A great way to test any offline scenario is to use the oldest trick in the book: airplane mode. By default, airplane mode turns off both wifi and cellular data access. Try enabling airplane mode, opening your app fresh, and test out all of the relevant functionality of your app.

If you need to detect offline status in code to toggle app functionality, NativeScript provides you with a [connectivity API](https://docs.nativescript.org/cookbook/connectivity):

	var connectivity = require("connectivity");
	var connectionType = connectivity.getConnectionType();
	
	switch (connectionType) {
	    case connectivity.connectionType.none:
	        //console.log("No connection");
	        break;
	    case connectivity.connectionType.wifi:
	        //console.log("WiFi connection");
	        break;
	    case connectivity.connectionType.mobile:
	        //console.log("Mobile connection");
	        break;
	}

Maybe someday in the not-so-distant future, we will be able to check Facebook and post to Instagram from the bottom of the Mariana Trench. Until then, let's make sure our apps work anywhere and everywhere without a hitch!

[![nativescript banner](nativescript-banner.jpeg)](https://www.nativescript.org/)