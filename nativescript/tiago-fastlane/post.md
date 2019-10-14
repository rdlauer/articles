# Automatic NativeScript App Deployments with fastlane

If I had to point out the single most unpleasant task of the otherwise exciting life of the NativeScript developer, I would say: deployments! Oh, how I despise them...

From Apple's provisioning profile hell to the ever-changing Google Play Console, deploying an app always feels boring, cumbersome, and just a waste of time you could be using developing awesome features for your awesome app. During the lifespan of a project, an app can be deployed dozens (if not hundreds!) of times between testing versions, beta versions, bug fixes, and new features. Wouldn't it be great if there was a way to make deployments automatic so we can ship the latest version of the app to the hands of our teammates and users whenever we want? Well...there is, thanks to [_fastlane_](https://fastlane.tools/).

_fastlane_ is a tool to automate deployments and releases for iOS and Android apps. With the right configuration, it can be used to automate NativeScript deployments. As you can see by the length of this article, this it's not trivial to set up...but, if your project lasts long enough, it will save you a lot of time.

We will go through the steps required to configure _fastlane_, and explain each one so you know how to tweak it for your project. This article is the result of several hours of experimentation until I reached with a configuration that makes sense for my projects, with a decent level of concern about managing secrets within a development team. Again, feel free to adapt it to your project and organization.

> Disclaimer: In this article we will assume you have deployment figured out for both [iOS](https://docs.nativescript.org/tooling/publishing/publishing-ios-apps) and [Android](https://docs.nativescript.org/tooling/publishing/publishing-android-apps). We are not showing how to deploy apps, just how to automate those deployments.

As an example, we will create a _fastlane_ configuration to a demo project called HubbyChef.

## Getting Started with _fastlane_

We will start by installing _fastlane_, which can be done with the command:

	brew cask install fastlane

> For more installation options and troubleshooting, check the [_fastlane_ documentation](https://docs.fastlane.tools/#getting-started).

We cannot use the _fastlane_ command `fastlane init` to get started because the NativeScript project folder structure is different from the "normal" native app. Instead, we will manually create and edit the configuration files.

### Fastfile (`fastlane/Fastfile`)

Create the folder `fastlane/` and the file `Fastfile` in it.

	mkdir fastlane
	touch fastlane/Fastfile

The `Fastfile` is the main _fastlane_ configuration file. This is where we add our _lanes_. You can think about a _lane_ as a set of tasks. _Lanes_ include [actions](https://docs.fastlane.tools/actions/) and other _lanes_. You can also group _lanes_ under a _platform_.

Let's see how this all pans out by creating the top structure for our `Fastfile`:

	fastlane_version '2.131.0'
	
	desc 'test lane'
	lane :test do
	  print "TEST SUCCESSFUL!"
	end
	
	platform :ios do
	
	 desc 'Fetch certificates and provisioning profiles'
	 lane :certificates do
	 end
	
	 desc 'Build the iOS application.'
	 lane :build do
	 end
	
	 desc 'Ship to Testflight.'
	 lane :beta do
	 end
	
	end
	
	platform :android do
	
	 desc 'Build the Android application.'
	 lane :build do
	 end
	
	 desc 'Ship to Playstore Alpha track.'
	 lane :alpha do
	 end
	
	end

The first line (`fastlane_version '<x>'`) will make sure your teammates are not using an outdated _fastlane_ version. Then, we created a test _lane_ `test`, just to see if _fastlane_ is working. You can test it with the command `fastlane test`. It should result in `TEST SUCCESSFUL!` printed in the console.

Then, we added two _platforms_: `ios` and `android`. For the `ios` platform, we will have the _lanes_ `certificates`, `build`, and `beta`. For `android`, the _lanes_ `build` and `alpha`. The `desc`s above each _lane_ describe what they will do. We will later invoke these _lanes_ with `fastlane <platform> <lane>`, e.g. `fastlane ios build`.

Running `fastlane lanes` will show a summary of all the available _lanes_:

![fastlane lanes result](fastlane_lanes.png)

### Appfile (`./fastlane/Appfile`)

Create the `Appfile` inside the `fastlane` folder. This file will store information about the app, as you can see below:

	app_identifier "dev.tiagoalves.hubbychef"
	apple_id "tralves@tiagoalves.dev"
	team_id "xxxxxxxxx"

### .env.default (`./.env.default`)

It is not safe to store secrets like passwords and API keys in the _fastlane_ configuration files directly because those files are part of the project and should be pushed code to version control along with the. Instead, we will use [`dotenv`](https://docs.fastlane.tools/best-practices/keys/#dotenv) and store all sensitive information in a `.env.default` file. Just don't forget to `.gitignore` it.

This file contains a set of key-value entries such as:

	MATCH_PASSWORD="xxxxxxxxx"

We can use this value in the `Fastfile` with `ENV["MATCH_PASSWORD"]`, but sometimes actions will just use these values under the hood.

## iOS

We will now build our iOS configuration, starting with the hardest part: code signing.

### Code signing

Allow me to go ahead and recommend the [`match` approach](https://docs.fastlane.tools/actions/match/). You can read all about this concept [here](https://codesigning.guide/), but in short, this what `match` will do for you:

- Generate the necessary certificates and provisioning profiles automatically (no need to access the Apple Developer Account page);
- Store the certificates and provisioning profiles in a repository (Git or Google cloud storage). All files will be encrypted as an additional security layer;
- Easier setup for deployments in a new machine or onboarding a new team member. `match` will take care of installing everything as part of the build process;
- Integrates with `fastlane` builds;

You can read all about `match` in the [official documentation](https://docs.fastlane.tools/actions/match/). Here, we will show the basic steps to get it working.

> Tip: The `match` documentation suggests creating _"a new, shared Apple Developer Portal account, something like `office@company.com`"_. This will make sharing access across the team much easier.

**1) Install `match`**

Run the command:

	fastlane match init

This command will ask about the storage method for the certificates and provisioning profiles. I recommend using GIT and creating a new private repository for the certificates (e.g. `https://github.com/tralves/hubbychef-certs`). The command will create the file `fastlane/Matchfile`.

**2) Configure `match`**

Edit the file `fastlane/Matchfile` with your project and Apple account information. This is how my `Matchfile` looks like:

	git_url("https://github.com/tralves/hubbychef-certs")
	
	storage_mode("git")
	
	type("development") # The default type
	
	app_identifier("dev.tiagoalves.hubbychef")
	username("tralves@tiagoalves.dev") # Your Apple Developer Portal username
	
	team_id('xxxxxxxxxx')
	team_name('Tiago Alves')

Add the repository encryption password to `.env.default` so you don't have to supply it every time you deploy.

	MATCH_PASSWORD="xxxxxxxxx"

**3) Generate the certificates and provisioning profile**

You can now generate new certificates and provisioning profiles with `match`.

> Tip: Before doing this step, make sure you remove all Apple Developer certificates for this account in your machine's Keychain Access. `match` will create and install new certificates that may conflict with existing ones.

To do so, run:

	match development
	match appstore

This will create the development and distribution certificates and their respective provisioning profiles. It will also install them on your machine. If all goes well, you can now open the project in Xcode and use the new provisioning profiles. You can also see these files commited in the GIT repository created earlier. Like magic!

**4) Configure the `certificates` _lane_**

Now you can configure the `certificates` _lane_ in your `Fastfile`:

	desc 'Fetch certificates and provisioning profiles'
	lane :certificates do
		match(type: 'development')
		match(type: "appstore")
		# match(type: "adhoc")
	end

With this _lane_, you can install all certificates with `fastlane ios certificates`.

### Build step

With all certificates and provisioning profiles in place, we can now work on the build step. Here, we will create an `.ipa` signed with the distribution profile that we can ship to Testflight or the App Store later. This is how my `ios build` _lane_ looks like:

	desc 'Build the iOS application.'
	lane :build do
	    sh("tns", "prepare", "ios", "--release", "--clean", "--env.production")
	
	    match(type: "appstore")
	
	    build_app(
	      scheme: "HubbyChef",
	      workspace: './platforms/ios/HubbyChef.xcworkspace',
	      export_method: "app-store"
	    )
	end

The first action in this _lane_ runs the command `tns prepare ios --release --clean --env.production`, which is the command we would type in a manual deployment.

The second action, `match(type: "appstore")`, makes sure the distribution certificate and provisioning profile are installed and sets them to be used in the next action.

Lastly, the [`build_app` action](https://docs.fastlane.tools/actions/build_app) compiles and signs the `.ipa`. Mind the parameters in the action:

- `scheme`: the build scheme as it appears in the top bar in Xcode, usually, the name of the app:
  ![xcode scheme](xcode_scheme.png)
- `workspace`: the path to the project `.xcworkspace` file;
- `export_method`: method to export the archive. In this case, we want `app-store`.

At this point, we can generate the signed `.ipa` with the command:

	fastlane ios build

If all goes well, you should see the `.ipa` in the root folder of your project.

### Shipping to Testflight

The final step is shipping the `.ipa` to Testflight. Check out the `ios beta` _lane_:

	desc 'Ship to Testflight.'
	lane :beta do
	    build
	
	    changelog_from_git_commits
	
	    upload_to_testflight(
	      beta_app_feedback_email: "tralves@tiagoalves.dev",
	      beta_app_description: "App for Hubbies trying to learn how to cook.",
	      demo_account_required: false,
	      distribute_external: true,
	      groups: ["beta testers"],
	      notify_external_testers: true,
	      beta_app_review_info: {
	        contact_email: "tralves@tiagoalves.dev",
	        contact_first_name: "Tiago",
	        contact_last_name: "Alves",
	        contact_phone: "+351 9xxxxxxxx",
	        demo_account_name: "",
	        demo_account_password: "",
	        notes: "<3 Thank you for reviewing!"
	      },
	    )
	end

The _lane_ starts by invoking the `build` _lane_ we defined earlier so we always have a freshly built app to send to Testflight.

Then, I like to use the [action `changelog_from_git_commits`](https://docs.fastlane.tools/actions/changelog_from_git_commits/). By default, this action grabs all the commit messages since the last GIT tag and uses them as the changelog that will be used when submitting the app to Testflight. This way, your beta testers will know what's new in each version, and you don't have to do anything for it (except writing proper commit messages...).

The last action, [upload_to_testflight](https://docs.fastlane.tools/actions/upload_to_testflight/) will:

1. upload the `.ipa` to the App Store Connect;
2. wait for the app to be processed;
3. add the build to Testflight;
4. assign to the desired test groups (in this case, the group "beta testers");
5. submit the app for review;

If you ever did this process manually, you will identify the parameters in this action with the form fields you otherwise would have to fill over and over again. With this configuration, all you have to do is:

	fastlane ios beta

You can also use _fastlane_ to submit the app to the App Store using the action `upload_to_app_store`. You can read all about it [here](https://docs.fastlane.tools/actions/upload_to_app_store/). I am not creating that _lane_ in this tutorial because it isn't something I tried myself, as I rather have more control over this step. It should be pretty similar to using `upload_to_testflight`, though.

## Android

We are only halfway through! Time to create the _fastlane_ Android configuration.

### Code signing

You probably already have `.keystore` file for your project. If not, see [here](https://docs.nativescript.org/tooling/publishing/publishing-android-apps) how to create one.

We put the keystore in `certs/hubby-chef-prod.keystore` and added its secrets to the `.env.default`:

	KEYSTORE_PASSWORD="xxxxxxxxxxxxx"
	KEYSTORE_ALIAS="dist"
	KEYSTORE_ALIAS_PASSWORD="xxxxxxxxxx"

### Build step

To build the Android app need a _lane_ that runs the `tns` command to create a signed production build:

	desc 'Build the Android application.'
	  lane :build do
	    sh("tns", "build", "android", "--release", "--clean", "--env.production",
	        "--key-store-path", "../certs/hubby-chef-prod.keystore",
	        "--key-store-password", ENV["KEYSTORE_PASSWORD"],
	        "--key-store-alias", ENV["KEYSTORE_ALIAS"],
	        "--key-store-alias-password", ENV["KEYSTORE_ALIAS_PASSWORD"]
	      )
	end

And we are done! You can now run `fastlane android build` and, in the end, you should see the `app-release.apk` under `platforms/android/app/build/outputs/apk/release/`.

### Ship to the Play Store alpha track

We need to set up a Google Developers Service Account. To do that, follow the steps bellow (or check the [_fastlane_ documentation](https://docs.fastlane.tools/actions/upload_to_play_store/#setup)):

- Open the Google Play Console;
- Click the "Settings" menu entry, followed by "API access";
- Click the "CREATE SERVICE ACCOUNT" button;
- Follow the "Google Developers Console" link in the dialog, which opens a new tab/window:;
- Click the "CREATE SERVICE ACCOUNT" button at the top of the Google Developers Console;
- Provide a Service account name (e.g. `hubbychef-manager`);
- Click "Create";
- Click "Select a role" and choose "Service Accounts" > "Service Account User";
- Click "Continue";
- Click "Create key";
- Check the "Furnish a new private key" checkbox;
- Make sure "JSON" is selected as the "Key type";
- A JSON file will be downloaded;
- Click on "Grant Access" for the newly added service account;
- Choose "Release Manager" (or alternatively "Project Lead") from the "Role" dropdown;
- Click "ADD USER" to close the dialog;

Then, copy the JSON file contents to `.env.default`, like so:

	PLAYSTORE_JSON_KEY_DATA='
	{
	  "type": "service_account",
	  "project_id": "api-642090...",
	  "private_key_id": "11d039fd....",
	  "private_key": "-----BEGIN PRIVATE KEY-----\nMI...",
	  "client_email": "hubbychef-manager@api-6420905...",
	  "client_id": "114219...",
	  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
	  "token_uri": "https://oauth2.googleapis.com/token",
	  "auth_provider_x509_cert_url": "https://www.googleapis...",
	  "client_x509_cert_url": "https://www.googleapis.com/..."
	}

We can now create the _lane_ in the `Fastfile`:


	desc 'Ship to Playstore Alpha.'
	  lane :alpha do
	    build
	
	    changelog_from_git_commits
	
	    upload_to_play_store(
	      track: 'alpha',
	      track_promote_to: 'alpha',
	      json_key_data: ENV["PLAYSTORE_JSON_KEY_DATA"],
	      apk: './platforms/android/app/build/outputs/apk/release/app-release.apk'
	    )
	end

As we did for iOS, we start by calling the `build` _lane_. Again, we will use the `changelog_from_git_commits` to generate the changelog.

The build is then shipped in the action `upload_to_play_store`. Here, we are sending the build to an "Alpha" track, but you can send it to the Beta or Production. Check the [`upload_to_play_store`'s documentation](https://docs.fastlane.tools/actions/upload_to_play_store/) for more details.

## The Workflow

Allow me to add another note about my deployment workflow with _fastlane_. These are the steps I take to deploy a new version:

1) Increment the version in `package.json`.

There is a problem with incrementing the version, though: Apple will take hours or even days to review your app, even if you are just deploying to Testflight. My alternative is using [this NativeScript plugin](https://github.com/tralves/nativescript-dev-version) that allows you to add a `versionNumber` parameter in `package.json`. This number will be the Build number on iOS. If you only increment this value, your app will be approved for testing immediately. The plugin also makes sure you get an incremental `versionCode` on Android.

> Warning: The _fastlane_ action `increment_build_number` will not work well with the NativeScript app generation flow.

2) Run the fastlane commands:

	fastlane ios beta
	fastlane android alpha

3) Create a git tag

This will make sure the action `changelog_from_git_commits` will only grab the right commits the next time we deploy.

## All in All...

_fastlane_ is a great tool! In this article we just scratched the surface on what _fastlane_ can do for you. If there is something you would like to attach to the build process, chances are there is an [action](https://docs.fastlane.tools/actions/) for that. I am talking about things such as integrating automatic tests, taking screenshots, slack bot/report, all sorts of CI integrations, etc.

I know all this work looks daunting, but it will be a fraction of the time it took me to figure this out! It will certainly save you time and patience. Deployments will be a breeze to do... You will end up deploying more often, thus making your users, teammates, QA people, and boss happier. Enjoy!
