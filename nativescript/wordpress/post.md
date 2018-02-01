# Integrating WordPress Content in a NativeScript App

Did I throw you for a loop there? Yes, WordPress is for managing web content and NativeScript is a great framework for building native mobile apps. So what do the two have in common?

As with any great relationship, it all started with a RESTful API.

IMAGE

WordPress includes REST API endpoints for WordPress data types, providing web (and now mobile) developers the ability to interact with web content in new and exciting ways.

## But Why?

I imagine if you are here, you have an existing WordPress site with weeks, months, or even years worth of content. What about re-purposing that content in a mobile app?

> If you're an Angular developer and interested in the NativeScript + Angular web/mobile code sharing story, be sure to check out this webinar on YouTube.

Even though WordPress is built on PHP (the horror!), the provided REST API endpoints are language-agnostic. Any language/framework that consumes JSON will happily consume what WordPress provides.

## Understanding the WordPress API

While a deep dive of the WordPress API is out of the scope of this article, suffice it to say they are well documented over at wordpress.org.

However, the app we are going to build with NativeScript is going to leverage WordPress categories, content titles, and content HTML. We will use that to create a relatively simple proof-of-concept app to show off how we can re-use some existing web content in a native mobile app.

- introduce demo data
- start app in sidekick
- add listview
- add webview thingy?
- consume categories
- consume titles
- consume web content
- show off app
