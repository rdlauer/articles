# Building Services That Work in All Apps. No Silos.

A silo is a popular metaphor that refers to a system or process that operates in isolation. In the software world, a siloed service or product is one that only works within its own ecosystem, and does not integrate with other services.

When building software in this new mobile world, we need services that work everywhere - regardless of the platform we develop on. We no longer have the luxury of developing for one platform, instead having to manage an ever-changing landscape of mobile and desktop environments. Siloed services are not flexible enough to meet this need.

Rather than talking about it, let's jump in a see how powerful services can be when they are available to any app. In this article we'll integrate [Telerik Backend Services](http://www.telerik.com/backend-services) into applications on a variety of platforms.

Let's start by looking at what we'll build.

### Our Restaurant Chain

For the purposes of this article we'll play the role of a hypothetical restaurant chain. The chain wants to collect feedback from customers so they know where they need to improve.

The chain has iOS and Android apps, as well as a website - and they want to include a survey on all three. From an implementation standpoint, this requirement immediately invokes a few questions. Where will we store this data? How can we aggregate the data so it's useful? How do we keep it secure?

The point of the Telerik Platform is to make this trivial for you as the developer. We can use Telerik Backend Services APIs to store and retrieve this data on any platform. Let's start by adding it to the Android and iOS apps.

### Adding Backend Services to a Hybrid App

The restaurant made the decision to build a [hybrid app](http://tech.pro/blog/1355/when-to-go-native-mobile-web-or-cross-platformhybrid) that targets both Android and iOS. The app lets the user find the nearest location and look through their menu, and is shown in the image below.

<img src="hybrid.png" alt="View of the restaurant's hybrid app in AppBuilder">

*Note: This project is available in GitHub at [https://github.com/tjvantoll/Restaurant-Survey](https://github.com/tjvantoll/Restaurant-Survey). You can [clone it directly in AppBuilder](http://blogs.telerik.com/appbuilder/posts/13-12-19/git-started-with-github-in-mist) if you'd like to follow along.*

As you can see, the restaurant has the [HTML to create this survey](https://github.com/tjvantoll/Restaurant-Survey/blob/c734be1a7c4a3c8ffa20a9523f70b1087ea8c397/Restaurant%20App/index.html#L35-68) added, but they don't have it tied into a backend.

We'll take over from here, and tie this form into Telerik Backend Services The first thing we need to do is configure a new Backend Services project. We can do that with the *Create project* button within a Telerik Platform workspace.

<img src="backend-create.png" alt="Image of the button used to create a new project">

Then we need to give the new project a name and description and hit *Create project*.

<img src="backend-create-2.png" alt="The new project screen">

Now that we have a project, we have to add a type to receive our survey data. You can think of a Backend Services type like you would a schema from a more traditional database setup -  you're configuring a database in the cloud.

Next we'll click the types menu then the *Create a Content Type* button.

<img src="backend-menu.png" alt="Image pointing out the Types menu and new content type button">

From here we'll add three fields that correspond to the three items in our survey: appetizer, location, and rating.

<img src="backend-types.png" alt="Image showing three fields being added to our content type">

That's it! We have a backend. Everyone that has painstakingly setup and deployed a database should take a moment to revel in just how easy that was.

Anyways, our magical backend doesn't do us any good unless we do something with it. Since this hybrid app is still a web app, we need to download the JS SDK for Telerik Backend Services from the Platform's [download page](https://platform.telerik.com/#downloads/backendservices) and import it into the project. The image below shows the SDK imported into the hybrid app in Telerik AppBuilder.

<img src="backend-add-sdk.png" alt="Adding a script tag and the file into an AppBuilder project">

Now all we need is a little JavaScript to listen for the form submission, gather the data, and send it off to our backend in the cloud.

	new Everlive({ apiKey: "************" });

	$( "form" ).on( "submit", function( event ) {
	    event.preventDefault();

	    var data = {
	        location: $( this.location ).val(),
	        appetizer: $(this.appetizer ).val() == "yes",
	        rating: $( this.rating ).val()
	    };
	    Everlive.$.data( "Ratings" ).create( data );
	});

This is again nice and succinct. One line of code to tell Backend Services (e.g. the `Everlive` object) what our API key is, and another to create a new entry in the database.

If we submit the form, we can return our Ratings content type in our Backend Services project to see new entries appear live.

<img src="backend-data.png" alt="Display of new entries in Backend Services">

That's it! With a little configuration and a handful of lines of code, we have a backend to power our restaurant's survey. And since this app is a hybrid app, the survey can run on iOS and Android with this one code base.

But not all of the restaurant's customers use their native applications. Some use the restaurant's existing responsive web site, and the restaurant wants to incorporate their survey there as well.

### Web App

The restaurant rigs up the following HTML using Kendo UI and Bootstrap to collect the same data on their public site.

	<form class="well">
	    <fieldset>
	        <legend>Survey</legend>
	        <div>
	            <label for="location">
	                Which location did you visit?
	            </label>
	            <select name="location" id="location">
	                <option>North</option>
	                <option>South</option>
	                <option>East</option>
	                <option>West</option>
	            </select>          
	        </div>
	        <div>
	            <label for="appetizer">
	                Did you order an appetizer?
	            </label>
	            <select id="appetizer" name="appetizer">
	                <option>No</option>
	                <option>Yes</option>
	            </select>
	        </div>
	        <div>
	            <label for="rating">
	                Please rate your overall experience.
	            </label>
	            <input type="range" min="1" max="5" value="3" id="rating" name="rating">
	        </div>
	        <div>
	            <button class="k-button">Submit</button>
	        </div>
	    </fieldset>
	</form>

And here's what the form looks like.

<iframe width="100%" height="400" src="http://jsfiddle.net/tj_vantoll/AQnF3/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Now if you have developed forms like this before, you're probably used to a multi step process to configuring the backend; but since our data is in the cloud, there's no need to do any of this. In fact, we can use the exact same JavaScript to submit this data to our backend.

	new Everlive({ apiKey: "************" });

	$( "form" ).on( "submit", function( event ) {
	    event.preventDefault();
	    
	    var data = {
	        location: $( this.location ).val().toLowerCase(),
	        appetizer: $(this.appetizer ).val() == "yes",
	        rating: $( this.rating ).val()
	    };
	    Everlive.$.data( "Ratings" ).create();
	});

This is the power of services that work across multiple platforms. Instead of configuring server-side endpoints, writing database integrations, and possibly building an API - not to mention the maintenance and testing associated with all of these things - we simply wrote a few lines code to send our data to the cloud.

We're now aggregating data from the restaurant's iOS app, Android app, and public website - but we're not doing anything with it yet. Let's see what the restaurant might want to do with all of this data.

### Admin System

The survey has been out for a few months now and management wants to see results. They want to know know which locations are doing the best, whether their appetizer ad campaign has affected sales, and more.

Where will we put this information? Like most companies, our restaurant has an internal application that is only used by its employees. They place a variety of features - payroll, company memos, etc. - in this application, so it's a perfect spot to place our survey data.

But here's what's awesome: this application could be written in .NET, Java, PHP, or whatever - and because our data is in the cloud, we can access it. The following retrieves our rating data for the restaurant's internal app.

	new Everlive({ apiKey: "eVKxNui85A6TopjR" });
	Everlive.$.data( "Ratings" ).get();

That's it. The `get()` performs an AJAX call to retrieve the data from Backend Services. You can chain the `get()` with a `then()` method to provide callbacks for when the call works and when something goes wrong.

	Everlive.$.data( "Ratings" )
	    .get()
	    .then(
	        function( data ) {
	            console.log( "It worked! Here's the data.", data );    
	        },
	        function( error ) {
	            alert( "Sorry, loading the survey results failed." );
	        }
	    );

Now that we have data we can do whatever we'd like with it. The following builds uses Kendo UI DataViz to build some charts that management wanted.

<iframe width="100%" height="1300" src="http://jsfiddle.net/tj_vantoll/Hn3ax/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

*Note: This data is live. You can submit the web form from the previous example, refresh the page, and see your changes reflected in these charts.*

### This is Only the Beginning

Awesome, huh? And this is only the beginning.

* Going native? Backend Services has SDKs for [Android](http://docs.telerik.com/platform/backend-services/development/android-sdk/introduction) and [iOS](http://docs.telerik.com/platform/backend-services/development/ios-sdk/introdution).
* .NET developer? We have [an SDK](http://docs.telerik.com/platform/backend-services/development/net-sdk/introduction) for you too.
* Want to know what features your users are using? You can use [Telerik Analytics](http://www.telerik.com/analytics) to track what users are doing in your apps. It works [across multiple platforms](http://docs.telerik.com/platform/analytics/api/introduction) as well.

In today's mobile-driven software world, being flexible has never been more important. Using cloud services gives you this flexibility by removing the shackles typically associated with application development. Instead of worrying about where to put your data and how to access it - you can focus on the things that actually matter to your applications. And when you inevitably need to develop for a new platform, you can rest assured that your data will be available.

Interested in learning more? [Give the platform a try](https://platform.telerik.com). It's free to get started!
