# Creating Your First Native Mobile App with Visual Studio (Part 3)

[**<-- Read Part 2**](#)

In this blog post series, we are working on creating a truly native cross-platform mobile app with [NativeScript](https://www.nativescript.org/), Visual Studio, and SQL Server, from start to finish, using [Telerik AppBuilder](http://www.telerik.com/platform/appbuilder). In the previous post, we finished scaffolding our app using NativeScript layouts and added some mock data to see how it all worked. Now we need to focus on our backend with SQL Server and ASP.NET Web API!

By the end of this article we hope to:

- Set up a new [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2016) database;
- Create a simple RESTful service with [ASP.NET Web API](https://www.asp.net/web-api);
- Query the database and return a random quote to our app.

Let's get started!

## Setting Up a SQL Server Database

As a .NET developer, you're probably more than familiar with SQL Server. For this exercise, we are going to use the free SQL Server Express edition along with SQL Server Management Studio. While setting up a new instance of SQL Server is outside the scope of this tutorial, you can download and install both of these tools yourself:

- [Download SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-editions-express)
- [Download SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)

We are next going to create a very simple database with just one table that is going to store all of our Bill Murray quotes. The table will have three columns:

- **QuoteId** (the primary key)
- **QuoteText** (the text of the quote)
- **QuoteSource** (the movie source)

Using SQL Server Management Studio (SSMS), you can create a database and then add this table. The table should look like this when you are done:

![sql server table](part-3-table.png)

We're then going to manually populate this table with a handful of quotes from some of Bill's most popular movies. If you want, you can [download the Excel source file](part-3-bill-quotes.xls) and, inside SSMS, right-click on your database, go to **Tasks** --> **Import Data** to import this data from Excel.

## Why ASP.NET Web API?

Many of you also know the sordid history of web services on the .NET stack. From [SOAP](https://en.wikipedia.org/wiki/SOAP)-based services to [WCF](https://en.wikipedia.org/wiki/Windows_Communication_Foundation) to now [Web API](https://www.asp.net/web-api), .NET developers have numerous methods for exposing data via web services.

For this tutorial, we are going to stick with a Web API project. Why? It's arguably the easiest way to put together a RESTful service that leverages the commonly-used [JSON](http://www.json.org/) format.

As an example, if we were to create a SOAP-based service for this database, the response from our service might look something like this:

	<?xml version="1.0"?>
	<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://www.w3.org/2001/12/soap-envelope" SOAP-ENV:encodingStyle="http://www.w3.org/2001/12/soap-encoding" >
	   <SOAP-ENV:Body xmlns:m="http://www.myservice.com/billmurray">
	      <m:GetQuoteResponse>
			<m:QuoteId>1</m:QuoteId>
	     	<m:QuoteText>Back off man, I’m a scientist.</m:QuoteText>
			<m:QuoteSource>Ghostbusters</m:QuoteSource>
	      </m:GetQuoteResponse>
	   </SOAP-ENV:Body>
	</SOAP-ENV:Envelope>

Now if you are a computer, this is perfectly legible. However, returning our data in JSON format has the advantage of making your service both computer- AND human-readable:

	{
		"quote": {
			"QuoteId": 1,
			"QuoteText": "Back off man, I’m a scientist.",
			"QuoteSource": "Ghostbusters"
		}
	}

## Creating an ASP.NET Web API Project

Back in Visual Studio, right-click on your solution and select **New Project** to add a new web project. I'm going to choose to add a new "ASP.NET Web Application (.NET Framework)" to my solution. Those of you who are more adventurous may choose to create a [.NET Core](http://www.telerik.com/blogs/announcing-a-week-of-aspnet-core) version of this, but I'm going to play it safe.

As I set up my project, I'm only going to include the Web API template, since this project will have no user interface at all:

![asp.net web api template](part-3-web-api.png)

Now you'll notice in your solution explorer that the Web API template provides A LOT of boilerplate code. You can delete extraneous files if you want, but I'm going to keep this simple and just add a few files where we need them.

Open up your `Global.asax.cs` file and add the following two lines of code to the `Application_Start` method:

	RouteTable.Routes.MapHttpRoute("ActionApi", "api/{controller}/{action}/{id}", new { id = RouteParameter.Optional });
	GlobalConfiguration.Configuration.Formatters.XmlFormatter.SupportedMediaTypes.Clear();

The first line maps an HTTP route to a controller we will be creating. This will let an action like "http://mywebsite.com/api/Quote/GetQuote" return data from the `GetQuote` action in the `Quote` controller.

The second line forces our Web API to disable the XML formatter and return valid JSON.

## Creating an ASP.NET Web API Controller

We will need to create a [controller](https://msdn.microsoft.com/en-us/library/dd410269(v=vs.98).aspx) to process the incoming request to get a random quote from our database. Controllers process incoming requests, handle user input and interactions, and execute appropriate application logic.

Right-click the `Controllers` directory and choose to add a new controller, using the **Web API 2 Controller - Empty** template.

![add controller](part-3-controller.png)

Name your controller **QuoteController** and open up the newly created `QuoteController.cs` file. As this is an empty template, let's add a "Hello World" function, just to make sure our Web API is returning data correctly. Go ahead and paste this into your `QuoteController.cs` file, after the opening `public class QuoteController : ApiController` line:

	[ActionName("GetQuote")]
	public string GetQuote()
	{
	    return "Hello NativeScript!";
	}

Now hit the magic Visual Studio green button to build and run this app! Your browser should open up and load the default page of your app. Simply append "/`/api/quote/GetQuote` to the URL and voila!

![hello nativescript](part-3-hello-nativescript.png)

Now all this does is just return a string. What we want to do is query our newly-created SQL Server database and return a random quote, and return the result in JSON format.

Before we get into the code of how to do this, we need to set up our data source in Visual Studio!

## Add an ADO.NET Entity Data Model

In order to access our database, we'll want to create a new ADO.NET entity data model, which is a key component of Microsoft's [Entity Framework](https://msdn.microsoft.com/en-us/library/aa937723(v=vs.113).aspx) (EF) technology. Instead of writing raw SQL, we are going to use EF to query our database.

Let's start by right-clicking on our Web API project, add a new **ADO.NET Entity Data Model**, and call it `QuoteModel`:

![ado.net entity data model](part-3-entity-model.png)

In the next step, be sure to choose **EF Designer from database**. This is because we are going to leverage a pre-existing database to create our data model. Entity Framework also allows for [code first models](https://msdn.microsoft.com/en-us/library/jj193542(v=vs.113).aspx) (i.e. create a database based on your C# classes).

![ef designer from database](part-3-ef-from-database.png)

In the next window you'll want to select your data source. In our case we already created a table in a SQL Server instance, so go ahead and add a new data source, choosing **Microsoft SQL Server**:

![data source](part-3-data-source.png)

In the subsequent connection properties window, be sure to specify the **server name**, **authentication type** (with credentials), and **choose the database** you are connecting:

![sql server connection properties](part-3-connection.png)

*It can't hurt to use the **Test Connection** button, just to make sure your connection is valid!*

Finally we just have to choose the database objects we want to use for our model. In our case, we only have one object! Go ahead and select the **Quote** table.

![database objects](part-3-db-objects.png)

Assuming everything worked correctly, you should now see the `QuoteModel.edmx` diagram open in Visual Studio, with one little entity:

![entity model](part-3-entity.png)

## Finish the ASP.NET Web API Controller

Phew! With that leg work out of the way, we can get back to code!

Open up the `QuoteController.cs` file (this is where we previously created the method that returned "Hello NativeScript!"). Get rid of that method and replace everything with this:

	using System;
	using System.Linq;
	using System.Web.Http;
	
	namespace WebAPI.Controllers
	{
	    public class QuoteController : ApiController
	    {
	        [ActionName("GetQuote")]
	        public System.Web.Mvc.JsonResult GetQuote()
	        {
	            BillMurrayEntities db = new BillMurrayEntities();
	
	            Random rand = new Random();
	            int toSkip = rand.Next(0, db.Quotes.Count());
	
	            Quote q = db.Quotes.OrderBy(r => Guid.NewGuid()).Skip(toSkip).Take(1).First();
	
	            return new System.Web.Mvc.JsonResult() { Data = q, JsonRequestBehavior = System.Web.Mvc.JsonRequestBehavior.AllowGet };
	        }
	    }
	}

What changed? Well, quite a bit, but the code is still relatively easy to follow:

- Instead of returning a `string` our `GetQuote` method now returns a `JsonResult`;
- We create a random number based on the count of records in our `Quotes` table;
- We query the table, taking only the first record returned after skipping a random number of rows;
- We return the record, properly JSON-formatted!

Hit the magic green button in Visual Studio again to build and run your project. When you access `/api/quote/GetQuote` you should now see a random quote appear!

![final json result](part-3-json.png)

The information we want is in the `Data` property of this object. What's with all of the other properties that we see? That's because we used `JsonResult` as the return type of our method. You can also use the `Json` function, but it requires a bit of a refactoring of this code. **There are a few different ways to do this, especially if you don't always know that you want to return JSON.**

## Deploy the Web API Project

The final step of setting up your backend will be to deploy it to a location that your mobile app can access. This could be a local deployment (with an network-accessible endpoint), a remote server, or a cloud provider like [Microsoft Azure](https://azure.microsoft.com/en-us/).

## What's Next?

Our NativeScript app is set up with mock data. Our backend is 100% complete. Next we need to wire our backend to our mobile app to make it all fully functional! In the final part of this series we will do just that, plus we will take our first look at [Telerik UI for NativeScript](http://www.telerik.com/nativescript-ui) and learn about native animations and styling our app with CSS.

[**Read Part 4 --&gt;**](#)