# Sending Text Messages to Phone Contacts using NativeScript

// https://developer.telerik.com/featured/sending-text-messages-to-phone-contacts-using-nativescript-on-ios/

The goal of this tutorial is to show how to pull a contact from your iOS contact list into a NativeScript app and prepare a text message for delivery. It leverages two NativeScript extensions that I have built: [nativescript-contacts](https://market.nativescript.org/plugins/nativescript-contacts) and [nativescript-messenger](https://market.nativescript.org/plugins/nativescript-messenger).

> This tutorial assumes you are already reasonably comfortable with NativeScript. If you’re new to NativeScript, I recommend following the [documentation on how to get started](https://docs.nativescript.org/).

Another thing to note is that, in order to actually send the SMS, you’re going to need to use a connected device. The emulators don’t support sending or testing of SMS. If you don’t have a device, you can still follow along and pull contacts from within an emulator and just trust that it will send an SMS if you dont have a device to connect with.

## Getting Started

From our command line/command prompt, we’ll start by creating a new app:

	tns create myApp
	
Next, cd into our app directory.

	cd myApp
	
Now we’ll install our plugins.

	tns plugin add nativescript-messenger
	tns plugin add nativescript-contacts
	
We’ll enter livesync mode while we build this.

	tns run ios|android
	
**TIP:** If you're testing this on Android, you'll want to add the following permission to your `AndroidManifest.xml` file:

	<uses-permission android:name="android.permission.READ_CONTACTS" />
	
You can also set the `android:targetSdkVersion="22"` to avoid a pesky permissions error.
	
## main-page.css

Before we begin building the core of the app, let’s overwrite the default CSS with this new bit for improved readability.

	StackLayout{
	 text-align:center;
	}
	
	Label{
	 font-size:16;
	 color: #555;
	}
	
	.btn {
	 margin:20 0;
	 font-size: 15;
	 background-color: #ff7070;
	 color: #fff;
	 padding: 10 15;
	 border-radius: 5;
	 horizontal-align: center;
	}
	
## main-page.xml

We’ll use main-page.xml as the core layout of our simple app example.

	<Page xmlns="http://schemas.nativescript.org/tns.xsd" loaded="pageLoaded">
	    <StackLayout>
	        <Button text="Choose a contact" tap="getContact" class="btn" />
	        <Label text="{{ contact_name }}" visibility="{{ contact_name ? 'visible' : 'collapse' }}" />
	        <Button text="{{ 'Send text to: ' + contact_name }}" class="btn" tap="sendMessage" visibility="{{ contact_name ? 'visible' : 'collapse' }}"/>
	    </StackLayout>
	</Page>
	
Let me explain whats happening above. We are using a stack layout to create a block for our inner elements (i.e. button, label and button).

The first button is going to open our contact list and bring a contact back for us to do something with. It is tied to a function we’ll create later. The label will display the selected contact’s name. The last button will fire an SMS with some predefined text that we’ll specify within a function in the JavaScript code later.

The latter two elements’ visibility is determined by whether or not a contact has been selected yet.

## main-page-model.js

This file contains our model, and is where we will store the data that will be displayed by our view above.

	// require the observable
	var observable = require("data/observable");
	
	// define our model
	var model = new observable.Observable({
	  contact_name: "",
	  phone: ""
	});
	
	// export it to the view
	module.exports = model;
	
## main-page.js

This is where the magic happens. First we need to set our dependencies and define the pageLoaded method, which defines what model we would like to use for the data. Our dependencies are the application module (which is used to grant us access to check what OS the user is running), the two plugins and our model.

	// lets require our dependencies
	var contacts = require('nativescript-contacts');
	var messenger = require('nativescript-messenger');
	var model = require("./main-view-model");
	
	// lets set up our model so its accessible in the page
	exports.pageLoaded = function(args) {
	  var page = args.object;
	  page.bindingContext = model;
	}
	
Next, let’s get some data from a contact in our device. We’ll start by creating a function that we can call from the view.

	exports.getContact = function(){
	  // code here
	}
	
### GETTING A CONTACT

We can now add our plugin code to this function:

	contacts.getContact().then(function(result){
	
	    var contact = result.data;
	    model.set("contact_name", contact.name.given);
	    // grab the first phone number in contact
	    if(contact.phoneNumbers.length > 0){
	    	model.set("phone", contact.phoneNumbers[0].value);
	    }    
	
	});
	
The contacts plugin brings back a data object with the contacts details.

	var contact = result.data;
	
Now we can assign the values to our model:

	model.set("contact_name", contact.name.given);
	if(contact.phoneNumbers.length > 0){
		model.set("phone", contact.phoneNumbers[0].value);
	}
	
A full list of other properties available can be [found on GitHub](https://github.com/firescript/nativescript-contacts).

### SENDING A TEXT MESSAGE

Let’s add another function below our `getContact()` function. This one is simple – all we want to do is grab the phone number from the chosen contact and call the `messenger.send(numbers, message, subject)` method to send a text.

	exports.sendMessage = function(){
	  var phonenumber = model.phone;
	  messenger.send(phonenumber, "my message", "my subject").then(function(args){
	    // after send you can do stuff
	    alert("Message sent to: " + phonenumber);
	  }, function (e) {
	    // if something went wrong
	    alert(e.toString());
	    console.log("Error occurred " + e);
	  }); 
	}
	
Again the message wont send on the emulator, you’ll get an alert saying “Error: You’re not able to send SMS messages. Please check device settings.” However, if you are able to test the app on an iPhone, it would open the compose message view and allow you to send a text message.

## Conclusion

If you’re a NativeScript developer, I hope you find these two plugins useful. If you’d like to see additional features or support, they are both open source and I invite anyone with the expertise to please contribute.