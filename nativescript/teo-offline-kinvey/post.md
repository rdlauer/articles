# Going Offline with NativeScript and Kinvey

As [Rob Lauer](https://twitter.com/RobLauer) already did write on [Going Offline with NativeScript](https://developer.telerik.com/products/nativescript/going-off-the-grid-with-nativescript), there are multiple ways of handling lack of Internet connectivity gracefully in your mobile app. When your app's data is stored in the cloud, it is up to you to manage what happens when the cloud is unreachable...or is it? Let's see how [Progress Kinvey](https://www.progress.com/kinvey) can help you handle such situations.

## Kinvey

Like Google's Firebase, Kinvey is a "Mobile Backend as a Service" (mBaaS) and offers great tooling for storing data, files, and integration with different data services. Besides, if you are sticking to Progress products and support, it is a natural step to take advantage of it.

### Offline Support Implementation

To be able to work with Kinvey, in the first place include the `kinvey-nativescript-sdk` dependency into your NativeScript project. Then, to get entities from a particular collection create a `DataStore` object:

	private constructionSitesStore = Kinvey.DataStore.collection<any>("ConstructionSites");

Refer the [Kinvey Data Store documentation](https://devcenter.kinvey.com/nativescript/guides/datastore#networkStore) for more detailed information.

> Start with Kinvey for free [here](https://console.kinvey.com/signup)!

In terms of offline support, there are three modes in which Kinvey operates:

- **Cache** (default) - data is stored locally once it's fetched, and re-used as soon as it is further requested. As soon as you update something, the SDK tries to persist it to the cloud.
- **Sync** - manually pull a copy of the data locally, work with it or modify it, and when needed "sync" it programatically to the cloud.
- **Network** - the SDK does not use any caching in this mode. Rather, it works directly with the cloud. Respectively, when offline, the app will not be able to interact with the data.

The default one (Cache) will work just fine in most cases. The way it works is you subscribe to a `find()` query, and the subscriber is called twice - when the data is retrieved from the local storage, and second time when it comes from the cloud: 


    allItems: BehaviorSubject<any> = new BehaviorSubject([]); // we subscribe to this object in some components, thus binding the UI to its updates

    updateData() {
        this.ensureLoggedIn().then(() => {
            this.load().subscribe(((constructionSitesRaw: Array<any>) => {
                const allConstructionSites = [];
                constructionSitesRaw.forEach((constructionSiteData: any) => {
                    constructionSiteData.id = constructionSiteData._id;
                    const constructionSite = new ConstructionSite(constructionSiteData);

                    allConstructionSites.push(constructionSite);
                });

                this.allItems.next(allConstructionSites);
            }), (err) => {
                console.log(err);
            });
        });
    }

	private load(): Observable<Array<any>> {
	    const sortByNameQuery = new Kinvey.Query();
	    sortByNameQuery.ascending("name");
	    const findQuery = this.constructionSitesStore.find(sortByNameQuery);
	
	    this.trackQueryOnlineStatus(findQuery); // pass the query to a service which (based on the result) evaluates whether we are online/offline
	
	    return findQuery;
	}

As you see in the code, we have a method `trackQueryOnlineStatus` which reports to a service for managing the online status:

	// subscribes to query results and reports whether it passed OK or failed due to network loss
	private trackQueryOnlineStatus(kinveyQuery: Observable<any>) {
	    kinveyQuery.subscribe(null,
	        (error: Kinvey.BaseError) => { 
	            console.log("Kinvey error:" + error);
	            if (error.message.indexOf("offline") !== -1) { // this error lets us know the issue is connectivity -> report offline status
	                this.connectivityStatusService.reportConnectivity(false);
	            }
	        }, () => { // Called after both sets of data (local and network) have been retrieved -> report online status
	            console.log("both sets of data (local and network) have been retrieved");
	            this.connectivityStatusService.reportConnectivity(true);
	        });
	}

On its own, the `connectivityStatusService` is a service that combines information from both Kinvey and NativeScript core's `connectivity` module. Thanks to its `getCurrentStatus()` method and `statusChangeEvent`, all components can be aware if the app is currently online or offline and can update accordingly:

	this.statusChangeSubscr = this.connectivityStatusService.statusChangeEvent.subscribe((status) => this.onConnectivityStatusChange(status));
	...
	onConnectivityStatusChange(newStatus) {
	    if (newStatus) {
	        this.refreshData();
	    }
	}

> The above code samples are from the newly added [Offline Support using Kinvey](https://play.nativescript.org/?template=play-ng&id=36AEWh&v=23) sample app.

It is a fully working data-driven app with offline support and nice design. To run the sample app in the NativeScript Playground you need to:

- Create a Kinvey app and hook it in `~/shared/config.ts`. Refer to [the docs](https://devcenter.kinvey.com/nativescript/guides/getting-started) if you are getting started.
- Create a User in the app, with hooked user and pass in `~/shared/config.ts`

Feel free to check it out!
