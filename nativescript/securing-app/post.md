# Tips on Securing a NativeScript App

When developing a mobile app are you regularly checking in (whether internally or with your team) on the security implications a certain feature may have? Do you hold regular security audits to validate the safety of your user's data? Are you still storing social security numbers in clear text?

If you answered yes to the last one, we need to have a talk.

When considering the security of a mobile app, we obviously need to take into account every place a third party could interact with the app. This not only means the actual usage of the app, but also what they can access with the APK/IPK file itself!

While NativeScript apps are truly native apps, they, like Cordova-based hybrid apps, do not pre-compile the app's XML/JS/CSS assets. This means your source code is exposed by default, which could allow malicious users to look into your app's plumbing to find vulnerabilities. Traditional native app developers aren't immune this either as it is incredibly easy to decompile both iOS and Android apps.

What do we do? Panic!? Nah. There is actually a lot we can do to help secure our NativeScript apps.

## Keep it on the Server

If the app you are developing will be used in an online (or even occassionally online) state, you should strongly consider keeping as much of your app's logic as possible on the server.

Mobile Baas?

## Secure Storage

## Utilize oAuth Authentication

## Secure the Source Code

jscrambler, encryption plugin

## SSL Everywhere

https by default, ssl pinning?

## Enterprise MAM/MDM

mobileiron plugin

## Touch ID? keychain

## Stay Up to date?

## PrivacyScreen plugin?