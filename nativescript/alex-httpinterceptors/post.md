# Tips on Showing Loading Indicators in NativeScript-Angular Apps

It's always a good idea to show a visual indicator to your users whenever your app is waiting for some data to be fetched from a backend. It's not something that is generally hard to do - you just show a loading indicator when start fetching, and hide it when data arrives. However, it might be a tedious task to actually do it in each and every page (or service) in your app.

Luckily Angular provides us with the tools to solve this problem once and for all in your entire app, using [HttpInterceptors](https://angular.io/guide/http#intercepting-requests-and-responses).

![xhr-interceptor](interceptor.jpg)

## The HttpInterceptor

By implementing an `HttpInterceptor` we will tap into the `HttpClient` pipeline and have control over the http requests and responses. You can use this for all sorts of stuff - like adding headers, caching, or redirecting on errors. In our case all we need to know is if there are requests currently waiting to be resolved.

We will also create a separate `HttpLoaderService` that will keep track of the number of active requests, so that we know whether we should show or hide the loading indicator. Here is the code for those two services:

**http-loader.service.ts**

	@Injectable({
	    providedIn: "root"
	})
	export class HttpLoaderService {
	    activeRequests$: BehaviorSubject<number>;
	    isLoading$: Observable<boolean>;
	
	    constructor() {
	        this.activeRequests$ = new BehaviorSubject(0);
	        this.isLoading$ = this.activeRequests$.pipe(
	            map(requests => requests > 0)
	        );
	    }
	
	    public onRequestStart() {
	        setTimeout(() => this.activeRequests$.next(this.activeRequests$.value + 1), 10);
	    }
	
	    public onRequestEnd() {
	        setTimeout(() => this.activeRequests$.next(this.activeRequests$.value - 1), 10);
	    }
	}

**http-interceptor.service.ts**

	import { Injectable } from "@angular/core";
	import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpResponse } from "@angular/common/http";
	import { Observable } from "rxjs";
	import { tap } from "rxjs/operators";
	import { HttpLoaderService } from "./http-loader.service"
	
	@Injectable()
	export class HttpInterceptorService implements HttpInterceptor {
	    constructor(private httpLoaderService: HttpLoaderService) {
	    }
	
	    intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
	        this.onStart(req.url);
	        
	        return next.handle(req).pipe(
	            tap((event: HttpEvent<any>) => {
	                if (event instanceof HttpResponse) {
	                    this.onEnd(event.url);
	                }
	            }, (err: any) => {
	                this.onEnd(req.url);
	            }));
	    }
	
	    private onStart(url: string) {
	        this.httpLoaderService.onRequestStart();
	    }
	
	    private onEnd(url: string): void {
	        this.httpLoaderService.onRequestEnd();
	    }
	}

## Registering

We should register our interceptor in the root module. Unfortunately we cannot do it simply by `providedIn: "root"` as http-interceptors are registered through a multi-provider using the `HTTP_INTERCEPTORS` token. The reason for this is you might have multiple interceptors in place. Registering looks like this:

	import { HTTP_INTERCEPTORS } from "@angular/common/http";
	
	@NgModule({
	    // ...
	    providers: [
	        {
	            provide: HTTP_INTERCEPTORS,
	            useClass: HttpInterceptorService,
	            multi: true
	        }]
	})
	export class AppModule { }

## Showing the Loading Indicator

The best place to put the indicator is the root component of our NativeScript app - this will make sure it will be active for all pages:

**app.component.html**

	<GridLayout>
	    <page-router-outlet></page-router-outlet>
	    
	    <!-- Busy indicator with an overlay -->
	    <GridLayout *ngIf="loaderService.isLoading$ | async" backgroundColor="#33252525" (tap)="true">
	        <ActivityIndicator width="100" height="100" busy="true" class="activity-indicator"></ActivityIndicator>
	    </GridLayout>
	</GridLayout>

Note the strange `(tap)="true"` code. This is optional - only if you want to block the user from interacting with the app while the indicator is up.

## Using HttpClient 

We don't have to do anything special from here on out. Just using the `HttpClient` in any of our services will notify the interceptor and the loading-service which will show the loading indicator.

**data-service.ts**

	export interface DataItem {
	    title: string;
	    body: string;
	}
	
	const URL = "https://jsonplaceholder.typicode.com/posts";
	
	@Injectable({
	    providedIn: "root"
	})
	export class DataService {
	    constructor(private http: HttpClient) { }
	
	    public getItems(): Observable<DataItem[]> {
	        return this.http.get<DataItem[]>(URL);
	    }
	}

## Non-HttpClient Requests

**Beware!** `HttpInterceptors` are only called when using the `HttpClient`. If you are using `fetch` or `xhr` directly you will bypass it. Also - if you are using a library or SDK that (via your backend) might not use `HttpClient` you will have to add some calls to `HttpLoaderService` so that the loading indicator is shown for http calls made by the library.

## Pre-fetching Data with Route Resolver

![wasn't sure which stick you threw - so i got them all](stick.jpg)

Another cool feature of Angular is [route-resolvers](https://angular.io/guide/router#resolve-pre-fetching-component-data). They allow you to pre-fetch some data **before** actually navigating to the page that needs the data. It spares you the humility of showing blank or partially rendered pages with no data. 

Implementing a resolver is pretty straightforward.

**data-resolver.service.ts**

	@Injectable({
	    providedIn: "root"
	})
	export class DataResolverService implements Resolve<DataItem[]> {
	    constructor(private dataService: DataService) {
	    }
	
	    public resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<DataItem[]> {
	        return this.dataService.getItems();
	    }
	}

Just don't forget to add the resolver to your `route-config`:

	{ path: "items", component: ItemsComponent, resolve: { items: DataResolverService }

It also works out nicely with the http-interceptors approach. The result is that you see the loading indicator on the current page, and when data is loaded navigation is triggered and the new page is shown in all its glory because all the data is already there. As a bonus you can have a loading-overlay preventing the user from making concurrent requests if tapping other buttons while data is being loaded.

## Summary

Quick summary of what we leaned:

- How to implement http-interceptor to track active http requests
- Showing application-wide loading indicator
- Using route-resolvers to pre-fetch data 

> **Bonus:** There was very little NativeScript-specific code in this example (actually only the markup in the `AppComponent`). You can reuse all of the things learned in your web or [code-sharing app](https://docs.nativescript.org/angular/code-sharing/intro) with NativeScript!

Here is a [NativeScript playground project](https://play.nativescript.org/?template=play-ng&id=pXQIML&v=7) that you can use to see all of this as well!