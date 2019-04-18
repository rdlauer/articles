# Dependency Versions in NativeScript Playground

[{N} Playground](https://play.nativescript.org) is one of the most popular ways to get started or quickly try something in NativeScript. When using it you need to have in mind there are some limitations as the Playground depends on apps released in Google Play and App Store. One thing to consider is that the versions of all the dependency packages that you run your app against are hardcoded in the NativeScript Preview application installed on your mobile device. Another one is that the templates and typings used on the website can differ from the ones that you have in your application.

Here are some more details about the two types of package versions:

## Playground Site Package Versions

With every major and minor release (and very rarely with patch releases) we update the templates used in the Playground site. This update includes updating the package versions in the template, the code, if there's a change in the used functionality of the template and the typings used by the Monaco editor for autocomplete. To view the concrete package versions you can download the package through the download button:

![](https://i.imgur.com/MMA5fiE.png)

Then you can explore the `package.json` file for all included packages and their versions.

## NativeScript Preview App Package Versions

As described above, the real packages that you see in action are the ones inside the concrete version of the Preview App you have installed on your mobile device. With every update of the site templates we release new versions of the Preview Apps, but it may take some time for them to appear in the stores, or you might have disabled the application's auto update. So it may happen that the versions differ from the packages in the template. To see the included packages and their concrete versions in your mobile app you need to connect it by scanning the QR code and look inside the **Devices** tab:

![](https://i.imgur.com/6U0N5Xf.png)

Note that if you use more than one device their versions might differ and you could see different behaviour on the different devices: ![](https://i.imgur.com/VScVgUk.png)

## Conclusion

To avoid compatibility issues make sure you have updated your Preview Apps to their latest versions before using the Playground. Also make sure that the functionality you use exists in the concrete package version used in the app.

*If you are interested in how the Playground work check [Architecture of the NativeScript Playground](https://www.nativescript.org/blog/architecture-of-the-nativescript-playground).*