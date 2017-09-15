# NativeScript UI 3.1 Released

Over the last two iterations, the [NativeScript UI](https://www.nativescript.org/ui-for-nativescript) team has been focused on addressing customer feedback and polishing the existing components. We hit several milestones regarding the number of issues that have been reviewed and addressed - and we are determined to continue in that direction. However, we also have some great announcements regarding new features in NativeScript UI 3.1!

**Let's look at what's new in NativeScript UI 3.1:**

## RadListView

We've introduced a new API in [RadListView](http://www.telerik.com/nativescript-ui/listview) that allows you to scroll to a given item by snapping to the item at different locations in the viewport. You can choose to use animations (or not) when scrolling as well. There is also a new event called `scrollDragEndedEvent` that is fired once the user stops dragging a finger. We have also extended the [item reorder API](http://docs.telerik.com/devtools/nativescript-ui/Controls/NativeScript/ListView/item-reorder) to allow for canceling the reorder operation.

## RadDataForm
 
The [RadDataForm](http://www.telerik.com/nativescript-ui/dataform) component received a lot of attention with this release! The full details are in the [release notes](http://docs.telerik.com/devtools/nativescript-ui/release-notes), but here is a brief summary of what's new: 

- A major new feature: support for asynchronous (Promise based) validation is available;
- New `Label` editor is now available;
- There is now an option to validate against a regular expression;
- Support for custom validators has been added;
- Control the position of editor labels.

## RadChart

The [RadChart](http://www.telerik.com/nativescript-ui/chart) component has also received some great updates. The `Trackball` behavior has been aligned across the iOS and Android platforms. We have implemented a new API to allow for customizing the `Trackball` content for each data point (see the [release notes](http://docs.telerik.com/devtools/nativescript-ui/release-notes) for more information). You can also control the width/length of the bars in a Bar chart by using the two new `minBarSize` and `maxBarSize` properties. 

## Squashed 🐛

On top of the new features and APIs there are many issues that have been resolved, coming from both our internal testing procedures and from [your feedback](github.com/telerik/nativescript-ui-feedback)! 

For the complete list of changes and updates in this release, please consult the full [release notes](http://docs.telerik.com/devtools/nativescript-ui/release-notes).

**Happy coding!**