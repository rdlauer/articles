# Offloading Tasks to Worker Threads with NativeScript

Performance. Performance. Performance.

When we talk to NativeScript developers, performance consistently ranks as one of the top reasons you choose NativeScript. But it's also something we always want more of. While mobile hardware continues to improve, there are always ways to improve the performance, and hence user experience, of the apps we create.

Aside from providing truly native UI across both iOS and Android, NativeScript has some additional tricks up its sleeve to allow you to customize your app for unique scenarios and squeeze even more out of those precious clock cycles.

Let me introduce you to worker threads.

## Worker/Service Worker

Traditionally known in the web world as service workers, worker threads allow you to take a single-threaded environment like NativeScript provides and turn it into a multi-threaded one.

> {N} single vs multithread

Workers are effectively new processes spun up by the underlying platform to perform a function outside of the scope of the main process.

IMAGE

Service workers are quite popular with Progressive Web App (PWA) developers, as they allow for ??? and ???. But where PWA functionality diminishes is where NativeScript picks up the slack.

Let's take a look at some examples of when (and when not) to use workers with a NativeScript app.

## When to Use a Service Worker

Virtually any task that can run outside of synchronous UI operations is, at least in theory, a candidate for service workers.

> Before you start thinking a worker is a silver bullet for complicated performance issues, make sure you read through the next section :)

The most simple worker thread does...

Background tasks are great examples of when workers can be beneficial. Take analytics for instance. If you are heavy user of Google Analytics, you may find yourself measuring virtually every user action, page view, feature view, and remote service call in your app. Even though these calls will run asynchronously, they can still have a negative impact on the main thread.

Another good example is image processing - a CPU-intensive task on its own, made much more complicated when you mix it into the UI thread!

In this example from the NativeScript docs...

## When NOT to Use a Service Worker

It's important to keep in mind that every time you spin up a new worker thread, you are adding to the processing and memory footprint of your app. This means that should you spin up too many at once or use them in the wrong situations, the performance of your app may actually decrease.

If you think a worker thread will help you process input from a form, or displaying a chart, or many other tablestakes app features, think again. The NativeScript framework has already been optimized for the vast majority of these scenarios.

Your best bet is always to measure the functional performance of your app on a variety of both iOS and Android physical devices. Going even further you can profile your apps using the debugging tools provided with the NativeScript CLI.

