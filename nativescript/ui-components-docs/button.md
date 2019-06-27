# Button

`<Button>` is a UI component that displays a button which reacts to a user gesture.

![](https://docs.nativescript.org/img/theme/btn-primary-ios.png) ![](https://docs.nativescript.org/img/theme/btn-primary-android.png)

## Usage

### Core (Tab)

	<Button text="Hello World" class="btn btn-primary" id="myButton" tap="onTap" />

### Angular (Tab)

	<Button text="Hello World" class="btn btn-primary" id="myButton" (tap)="onTap($event)"></Button>

### Vue (Tab)

	<Button text="Hello World" class="btn btn-primary" id="myButton" @tap="onTap" />

## Theming

| Class | Description |
|------|-------------|
| `btn` | A class name that applies the base styles of the NativeScript core theme, including dimensions and spacing.
| `btn-primary` | A class name that applies the primary color pattern of the theme to the button.
| `btn-outline` | A class name that makes a button appear with a border and a transparent background.
| `btn-rounded-sm` | A class names that makes a button appear with a small rounded corners.
| `btn-rounded-lg` | A class name that makes a button appear with large rounded corners.
| `btn-active` | A class name that makes a button appear highlighted when tapped.

*Example of `class="btn btn-outline"`:*

![](https://docs.nativescript.org/img/theme/btn-outline-ios.png) ![](https://docs.nativescript.org/img/theme/btn-outline-android.png)

## Properties

| Name | Type | Description |
|------|------|-------------|
| `text` | `String` | Sets the label of the button.
| `textWrap` | `Boolean` | Gets or sets whether the widget wraps the text of the label. Useful for longer labels. Default value is `false`.

## Events

| Name | Description |
|------|-------------|
| `tap` | Emitted when the button is tapped.

### Core (Tab)

	export function onTap(args: EventData) {
	    const button = <Button>args.object;
	    button.text = `Tapped ${count} times`;
	}

### Angular (Tab)

	onTap(args: EventData) {
	    let button = <Button>args.object;
	
	    this.counter++;
	    alert("Tapped " + this.counter + " times!");
	}

### Vue (Tab)

	????

## API Reference

API reference for the [Button Class](http://docs.nativescript.org/api-reference/classes/_ui_button_.button.html).

## Native Components

| Android | iOS |
|---------|-----|
| [`android.widget.Button`](https://developer.android.com/reference/android/widget/Button.html) | [`UIButton`](https://developer.apple.com/documentation/uikit/uibutton)

## Tips & Tricks

To style and customize the `Button` widget use the conventional CSS properties and Icon Fonts. The widget also supports the CSS pseudo-selector `active`, which is triggered while the button is being pressed.

To disable a button use the [`isEnabled`](https://docs.nativescript.org/api-reference/classes/_ui_button_.button#isenabled) attribute set to "false" in your markup instead of the usual `disabled` attribute.

**WARNING:** By default, iOS uses a delay before highlighting buttons used within `ScrollView` controls. Therefore, use caution when applying the `btn-active` class name to `<Button>` elements that are children of `ScrollView`. For more detail see [this Stack Overflow thread](http://stackoverflow.com/questions/7541159/is-it-possible-to-remove-the-delay-of-uibuttons-highlighted-state-inside-a-uisc).