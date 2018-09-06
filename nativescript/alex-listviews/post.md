# Managing Component State in NativeScript ListView

A while ago I blogged on using [Multiple Items Templates in a NativeScript ListView](https://medium.com/@alexander.vakrilov/faster-nativescript-listview-with-multiple-item-templates-8f903a32e48f) and briefly touched on the topics of **UI Virtualization** and **View/Component Recycling**. Looks like there are some hidden traps you can hit while developing apps with ListView related to this, especially if you are using Angular Components as items in you ListView and keep some state in the components.

We will take a deep dive into the problem and show some approaches to overcome it.

## The Scenario

To demonstrate the problem we will build an application that shows a list of items and we want to be able to select some of them.

We will be using a project that shares the code between Web and Mobile for this blog. The reasons:

1.  We can outline differences between Web and NativeScript templates.
1.  Code-sharing is now ridiculously easy thanks to angular-cli and @nativescript/schematics. [Read more about it in this awesome blog](https://blog.angular.io/apps-that-work-natively-on-the-web-and-mobile-9b26852495e7) by [Sebastian Witalec](https://medium.com/@sebawita)

Here is how the app will look like in browser and iOS simulator:

![](https://cdn-images-1.medium.com/max/800/1*mB7iHGWKWV1xUcUwaN8dsg.gif)

Each item in the list is rendered by an `ItemComponent` — having the current items as an `@Input` parameter . Here is the component class:

	@Component({
	  selector: 'app-item',
	  templateUrl: './item.component.html',
	  styleUrls: ['./item.component.css']
	})
	export class ItemComponent {
	  @Input() item: Item;
	  selected: boolean = false;
	}

Notice we are keeping the `selected` state as a field in the component. We are also using it in a couple of places in the template:

	// Mobile Template (item.component.tns.html)
	<StackLayout orientation="horizontal" class="item"
	    (tap)="selected = !selected">
	    <Label [text]="item.name"></Label>
	    <Label class="select-btn" 
	        [class.selected]="selected"
	        [text]="selected ? 'selected' : 'unselected'">
	    </Label>
	</StackLayout>
	// Web Template (item.component.html)
	<div class="item">
	  {{ item.name }}
	  <span (click)="selected = !selected" 
	    class="select-btn" 
	    [class.selected]="selected">
	    {{ selected ? 'selected' : 'unselected' }}
	  </span>
	</div>

The whole project along with branches for the different sections in the blog [lives here](https://github.com/vakrilov/fun-with-state-lists).

## Using Good Old *ngFor

We will start by showing all the items from the model in a *container* (a.k.a *smart*) component is using `*ngFor`:

    <app-item *ngFor="let item of items" [item]="item"></app-item>

Pretty straight-forward! This will render an `ItemComponent` for **each** item in the collection.

In the test-project there are 100 items generated and everything is super-fast for both web and mobile.

😈😈😈 Let’s try with more items 😈😈😈

The web app starts to get a considerable startup lag at 10K items. In the mobile items the threshold is much lower — around 2K. This is because the native
components rendered by IOS/Android are more expensive than browser DOM elements. If we make template more complex, these numbers will go down.

But...nobody puts 2000 items in a list you would say. And you are right. You will probably implement an infinite scroll with load on demand mechanics. The thing is — even then you will hit performance and memory problems as you are scrolling as `*ngFor` will instantiate more and more `ItemComponents` as you are scrolling down and pulling more data.

Here is the code so that you can play with it yourself — just tweak the [item.service.ts](https://github.com/vakrilov/fun-with-state-lists/blob/ngFor/src/app/home/item.service.ts#L13) to generate more items: [ngFor branch](https://github.com/vakrilov/fun-with-state-lists/tree/ngFor).

We can do better!

## Switch to ListView in NS

In NativeScript we utilize the native controls that do **UI Virtualization** and **View/Component Recycling. **This means only the UI elements for the items visible will be created and these UI elements will be recycled(or reused) to show the new items that come into view.

To start using `ListView` we just have to change the *ngFor-based template from above, to:

	<ListView [items]="items">
	  <ng-template let-item="item">
	    <app-item [item]="item"></app-item>
	  </ng-template>
	</ListView>

Great! Quick test shows that we can now scroll trough 100K `items` in the mobile app with no problems!

A simple counter in the `ItemComponent's` constructor shows that only **13 instances** are ever created. They are reused to show all items as you scroll.

## The Problem

Neat!...or is it? Let’s see what happens when we start selecting items:

![](https://cdn-images-1.medium.com/max/800/1*x_pyCAwiiPk-3n-YmN_R7g.gif)

Here we see **the problem** which is actually the reason for this post. I select the first 3 items. When I scroll down items 13,14 and 15 are also selected. Further down more items, that I have never even seen before, are also selected.

The reason for this is that when `ItemsComponents` are reused the state that is inside them is reused as well. There were only 13 components ever created so if you select 3 of them, you will see them popping again and again as you scroll.

When you think about it —with this implementation **you are actually selecting components not items**. And there is no longer 1 to 1 relation between those two collections: there are 100 (or maybe 😈100K😈) items and only 13 `ItemsComponent` instances.

Here is the branch in the repo with the problem at hand: [list-view-state-in-component branch](https://github.com/vakrilov/fun-with-state-lists/tree/list-view-state-in-component).

## The Solution

There are a couple of solutions, but they all ultimately boil down to:

> Move the view-state (the `selected` field from our example) **out of the component** and making the the component stateless.

We will refer with **view-state** (for the lack of better term) for all the information, that was not originally in the model, but is still used in the component templates and application logic. In our case this is the `selected` filed. This info might also be bound to any input view in you template.

*Note*: One alternative approach that comes to mind is to try to “clean” the components when they are reused. However, this means you will inevitably loose the state they were in. It is just not possible to store 100 items in 13 one-item boxes.

## Keep View-State in the Model

Maybe the most easy to implement solution, is just to add the view-state in the model items:

	export interface Item {
	  name: string;
	  selected?: boolean;
	}

You will have to change the component template to get/set the `selected` field from the `item`:

	<StackLayout orientation="horizontal" class="item"
	    (tap)="item.selected = !item.selected">
	    <Label [text]="item.name"></Label>
	    <Label class="select-btn" 
	        [class.selected]="item.selected"
	        [text]="item.selected ? 'selected' : 'unselected'">
	    </Label>
	</StackLayout>

Problem solved! To make things clearer. We went from ngFor with stateful components:

![ngFor with stateful components](https://cdn-images-1.medium.com/max/800/1*rFtP87eht43X-JfFUfPdwA.png)

to ListView (still ngFor in the Web version though) with stateless components:

![ListView with stateless component](https://cdn-images-1.medium.com/max/800/1*8AE3ZpXApEmJswQll2U7OQ.png)

**Note:** The web templates still uses `ngFor`. It works perfectly with the stateless version of `ItemComponent`. Here is the branch in the repo: [list-view-state-in-model branch](https://github.com/vakrilov/fun-with-state-lists/tree/list-view-state-in-model).

For simple cases this is a valid solution, but you might not want to mix view-state properties with the model. Or it might be the case that you get your model object directly from a service and you want to keep them “clean” from additional field, so that you can send them back at some point.

## Attach View-State to Item

Another approach would be to have the view-state as a separate view-state object and “attach” it to the model-object when it is used in the UI. This will give us some separation between model and view-state properties and an easy way to clean model objects if needed.

To make things even easier I have created a [TypeScript decorator](https://www.typescriptlang.org/docs/handbook/decorators.html) that will do the job for me. Here is how this goes:

1.  We decorate a dedicated view-state property in the component (let’s call it`vs` for short) with our special decorator: `@attachViewState`.
1.  We give the decorator a factory function to create the default view-state object for items. It will use it whenever it needs to create a view state object for an item.
1.  We give the decorator the name of the actual model property of in the component. Usually this is `@Input` property — in our case “item”.
1.  The decorator will create (using the factory) and “attach” a view-state object to each item passed to the component (“attach” is a fancy way of saying it will set a `"__vs"` property to the item).
1.  The decorator will also change the getter and setter for the `vs` property, so that they access the view-state object that lives inside the item. This will make it easier to use the view-state inside the component’s template.

Sounds complicated? Actually it is quite easy to use:

	interface ItemViewState {
	  selected?: boolean;
	}
	const ItemViewStateFactory = () => { return { selected: false } };
	@Component({ ... })
	export class ItemComponent {
	  @attachViewState<ItemViewState>("item", ItemViewStateFactory)
	  vs: ItemViewState;
	  
	  @Input() item: Item;
	}

And in the template we just use `vs` for the view-state props and `item` for data props:

	<StackLayout orientation="horizontal" class="item"
	    (tap)="vs.selected = !vs.selected">
	    <Label [text]="item.name"></Label>
	    <Label class="select-btn" 
	        [class.selected]="vs.selected"
	        [text]="vs.selected ? 'selected' : 'unselected'">
	    </Label>
	</StackLayout>

Here is also the code of the `@attachViewState`decorator (T being the type of the view-state object). There are also `getViewState` and `cleanViewState`
helper methods for getting and cleaning the view-state object from the model.

	const viewStateKey = "__vs";
	
	export function attachViewState<T>(attachTo: string, defaultValueFactory?: () => T) {
	    return (target: any, key: string) => {
	        const assureViewState = (obj) => {
	            if (typeof obj[attachTo][viewStateKey] === "undefined") {
	                // console.log("> creating default view sate");
	                obj[attachTo][viewStateKey] = defaultValueFactory();
	            }
	        }
	
	        // property getter
	        var getter = function () {
	            // console.log("> getter");
	            assureViewState(this);
	            return this[attachTo][viewStateKey]
	        };
	
	        // property setter
	        var setter = function (newVal) {
	            // console.log("> setter");
	            assureViewState(this);
	            this[attachTo][viewStateKey] = newVal;
	        };
	
	        // Delete property.
	        if (delete target[key]) {
	            // Create new property with getter and setter
	            Object.defineProperty(target, key, {
	                get: getter,
	                set: setter,
	                enumerable: true,
	                configurable: true
	            });
	        }
	    }
	}
	
	export function getViewState<T>(model: any): T {
	    return model[viewStateKey];
	}
	
	export function cleanViewState(model: any) {
	    return model[viewStateKey] = undefined;
	}

Again, the code is here: [list-view-state-in-model-decorator branch](https://github.com/vakrilov/fun-with-state-lists/tree/list-view-state-in-model-decorator)

Note: There are other tactics as well. For example:

* maintain a completely separated list of view-state objects in your container component and pass them both as inputs of the template
* “Wrap” your model item into view-model items using composition, thus keeping the model completely items untouched.

## Bonus (the Case for Stateless Components)

It’s worth noting that these solution work flawlessly in the Web version of our app where `*ngFor` is still used. In fact in many cases having stateless components will actually lead to better app architecture.

Here is an example. Consider the next feature in our app: we have to gather all selected items and show in a different view (or just `alert` them for now 😃).

If the the “selected” information resides inside the components we would have to either:

* Use `@ViewChildren` to query the components to figure out which are the selected items. 🤮Eew!🤮
* Expose some kind of event to notify whenever item is selected and handle it in the container component. Which means that we will hold the “selected” information on two different places (once in the the `ItemComponent` and once in the container-component). 🤮🤮Eeeew!🤮🤮

On the other hand, if you have a **stateless** `ItemComponent` and hold the state separately — you will have an easier time working with the data. Here is how the code looks like if you are using the “decorator” approach from above (we use the `getViewState` method from the helper util to get the view-state):

	// In container-component template (home.component.html):
	...
	<button (click)="checkout()">checkout</button>
	...
	// In container-component code (home.component.ts):
	...
	checkout() {
	  const result = this.items
	    .filter(item => {
	      const vs = getViewState<ItemViewState>(item);
	      return vs && vs.selected;
	    })
	    .map(item => item.name)
	    .join("\n");
	  alert("Selected items:\n" + result);
	}

The code of the final project: [master branch](https://github.com/vakrilov/fun-with-state-lists)

## Summary

Here are the key take-aways:

1.  When switching from `*ngFor` to `ListView` keep in mind, that it will **recycle** the components in your template. Any state inside them (all
non-`@Input` properties that are not bound in the template) will survive the recycling and will probably cause unwanted behavior.
1.  Consider using **stateless(a.k.a presentation) components**.** **It will save you from problems in **1.** as all state will be passed as input. It also
follows the [Smart Components vs Presentational Components](https://blog.angular-university.io/angular-2-smart-components-vs-presentation-components-whats-the-difference-when-to-use-each-and-why/) guides and will lead to better architecture of your app.
1.  **Bonus:** [Sharing code between web and mobile with NativeScript](https://docs.nativescript.org/angular/code-sharing/intro) is now really easy. Not really on the topic...but I’m excited about it and decided to *share* 😃😃😃
