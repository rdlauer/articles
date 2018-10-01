# Behold! NativeScript-Vue 2.0!

[NativeScript-Vue](https://nativescript-vue.org/), NativeScript’s implementation of its runtime with support for the Vue.js framework, has reached version 2.0 just seven months after we launched 1.0 on the stage of Vue.Amsterdam. The hard work of core contributor [Igor Randjelovic](https://twitter.com/igor_randj) and a trusty cohort of community members including Tiago Alves, Kamen Bundev, Manuel Saelices, Pascal Martineau, Vasil Trifonov, Rahul Kadyan, Ludovic Bois de Fer and many more has culminated in a really solid release for you, which includes:

**tl;dr; A new template, Sidekick integration, HMR, frame support, functional components, DevTools (wow!) and new docs**

> Join me for a webinar where we will dive into this release and have some fun with creating apps and using DevTools! Register [here](https://attendee.gotowebinar.com/register/1758384468554226433?source=blog).

## A Streamlined Template

When we first launched the standard system of creating a NativeScript-Vue app via the Vue CLI, the mobile app's folder structure somewhat resembled that of a web app scaffolded via the Vue CLI.

- The developer coded the app's screens and logic in the `/src` folder, which contained `/assets`, `/components`, and other parts of the app such as the Vuex store. 
- The `/template` folder contained the `/App_Resources` folder which is where a mobile app stores its icons and splash screens. This folder was necessary for the build process. 
- Finally, the built versions of the mobile app were stored in the `dist` folder which contains all the assets that will eventually be uploaded to the App Stores when the app is ready to be released. The app was run using `npm` commands, such as `npm run watch:ios` to enable LiveSync which watches for changes and refreshes the app in the simulator or on a device.

![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4D7A161827C68AFD3F40B1B2565EB2DC813C8158302C1845B2CF83F6D4528CA_1536788927995_Screenshot+2018-09-12+17.36.40.png)

*The old folder structure vs the new*
  
Now, the new Vue-CLI template is vastly simplified, aligning it with the folder structure of a standard NativeScript mobile app. Now, when you want to use this template

- Navigate to the folder where you want your app to live
- In your command line or terminal, type `vue init nativescript-vue/vue-cli-template myapp` in your terminal. 
- `cd myapp` 
- `npm install` the app's dependencies
- Run the app by using `tns` commands: `tns run ios/android` `--``bundle`. The bundle flag is necessary for webpack to build the app.

> You can even try the new templates with the new HMR (Hot Module Reload) capability which is starting to be supported by NativeScript. Ensure that you have the latest NativeScript installed (npm i -g nativescript@next) and then run `tns run ios/android —hmr)

Go ahead and try the template, you’ll be pleasantly surprised! And if you have been using the old template, we have prepared a handy [upgrade guide](https://nativescript-vue.org/en/docs/getting-started/upgrade-guide/) to move you from 1.3.1 to 2.0.

![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4D7A161827C68AFD3F40B1B2565EB2DC813C8158302C1845B2CF83F6D4528CA_1536943550535_init2.gif)

## Support for NativeScript 4.x `<Frame>` Changes

NativeScript 4.0 introduced a new `Frame` element, and introduced a way to change the root element of our applications, allowing for sharing common view elements across pages via navigating. Prior to 4.0 the root element was a Frame, which was implicitly created by NativeScript when an application started.

With these changes, we are no longer able to automatically create Frame and Page elements, so in NativeScript-Vue 2.0 you are required to explicitly add a `<Frame>` and a `<Page>` element to your template.

To keep the previous behavior of having a single root Frame, you can change your root Vue instance to have a `<Frame>`and a `<Page>` element.

**Example**

    // in prior versions
    // this automatically created a Page
    new Vue({
      template: `<Label text="Hello world"/>`
    }).$start()
    
    // in 2.0.0
    // the <Frame> and <Page> must exist in your template
    new Vue({
      template: `
        <Frame>
          <Page>
			<Label text="Hello world"/>
          </Page>
        </Frame>
      `
    }).$start()

This allows us to use a shared [RadSideDrawer](https://www.nativescript.org/ui-for-nativescript) across different pages for example:

    new Vue({
      template: `
        <RadSideDrawer>
          <StackLayout ~drawerContent>...</StackLayout>
          <Frame ~mainContent>
            <Page>
              <Label text="Hello world"/>
            </Page>
          </Frame>
        </RadSideDrawer>
      `
    }).$start()

In the new Vue-CLI template, however, you can simply rely on the provided enhancement to the Vue initialization already in place in `main.js`:

    new Vue({
      render: h => h('frame',[h(App)]),
    }).$start();

## Functional Components

All the elements in NativeScript-Vue are now functional vue components. This change should not affect performance, and it has been made to make debugging easier. We have been working on bringing VueDevtools to NativeScript-Vue, and with this change you are now able to see native elements in the devtools component tree. 

## New Docs
  
[NativeScript-Vue.org](https://nativescript-vue.org/) has been enhanced to provide documentation for this release. Earlier releases are also documented.
  
![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4D7A161827C68AFD3F40B1B2565EB2DC813C8158302C1845B2CF83F6D4528CA_1537195914398_Screenshot+2018-09-17+10.51.05.png)

## Sidekick Integration
  
With the new folder structure of the Vue CLI template, [NativeScript Sidekick](https://www.nativescript.org/nativescript-sidekick) integration is now possible for your NativeScript-Vue apps. Sidekick is an abstraction of the NativeScript CLI, allowing rapid development via a desktop app. Scaffold an app using the template, and open it in Sidekick. Select the device and ensure that the Webpack checkbox is checked, and you can start working on your app from within this environment.

![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4D7A161827C68AFD3F40B1B2565EB2DC813C8158302C1845B2CF83F6D4528CA_1536949569478_Screenshot+2018-09-14+14.13.33.png)

## Support for NativeScript UI components

Getting [NativeScript UI components](https://www.nativescript.org/ui-for-nativescript) compatible with Vue has been a long process, but we’re happy to present the RadSideDrawer and the RadListView, two of the most used and popular components, ready for you to use now with your NativeScript-Vue apps. 

![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4D7A161827C68AFD3F40B1B2565EB2DC813C8158302C1845B2CF83F6D4528CA_1537220566200_ns-ui-demo.gif)

If you’re interested or curious about this NativeScript UI demo code, go to the [ns-ui-vue-demo](https://github.com/msaelices/ns-ui-vue-demo/) repository and take a look around. This repo also enables the Vue DevTools, which is my final point:

## Vue DevTools!

Due to some very hard work by the good folks of the Vue core team, especially Guillaume Chau and Rahul Kadyan, with extra polish given by Igor, you can now use the fabulous Vue DevTools to debug your NativeScript-Vue apps!

![](https://d2mxuefqeaa7sj.cloudfront.net/s_B4D7A161827C68AFD3F40B1B2565EB2DC813C8158302C1845B2CF83F6D4528CA_1537431432221_DnU50_pUwAAw2A3.jpg)

Check out the guide on how to use DevTools on [NativeScript-Vue.org](https://nativescript-vue.org/en/docs/getting-started/vue-devtools/).

It’s been a wild ride for NativeScript-Vue. Come join us as we deepen our integration into the Vue ecosystem with cool projects like supporting Vue DevTools and creating mobile apps via a Vue-CLI plugin. Curious? Excited? [Join us on Slack](https://developer.telerik.com/wp-login.php?action=slack-invitation) in #vue and we’ll chat!