---
title: RxSwift.Lifecycle
author: hbanken
type: post
date: 2016-08-25T18:18:24+00:00
url: /2016/08/25/rxswift-lifecycle/
categories:
  - Coding
format: aside

---
Once in a while I&#8217;m reminded that good API design is of vital importance. Today was such a day. Everybody developing for either Android or iOS platforms knows that one often needs to run something when the view is loaded or when it will appear. Both platforms provide nifty &#8216;callbacks&#8217; for this:

<img class="alignnone wp-image-565 size-large" src="https://hermanbanken.nl/wp-content/uploads/2016/08/Jn6MZ-1024x811.png" alt="Android vs iOS lifecycle diagram; iOS: viewDidLoad, viewWillAppear, viewDidAppear, viewWillDisappear, viewDidDisappear, viewDidUnload; Android: onCreate, onStart, onResume, onPause, onStop, onDestroy" width="660" height="523" srcset="https://hermanbanken.nl/wp-content/uploads/2016/08/Jn6MZ-1024x811.png 1024w, https://hermanbanken.nl/wp-content/uploads/2016/08/Jn6MZ-300x238.png 300w, https://hermanbanken.nl/wp-content/uploads/2016/08/Jn6MZ.png 1436w" sizes="(max-width: 660px) 100vw, 660px" /> 

It is good practice to start listeners onStart and cleaning up onStop (or other equal pairs). This is all good, and if we want to share functionality we simply create a subclass of UIViewController or Activity and we put the shared logic there. Then comes along another &#8211; very unrelated &#8211; piece of shared functionality which we also need to put in there: oops! We have only single inheritance. Or, we want that same functionality in both Activity and Fragment, or UIViewController and UITableViewController. There are two problems:

  * We can not override the same method multiple times (without creating a dependency chain).
  * We can not put the logic in 1 shared subclass if there are multiple parents (possibly related by inheritance), since we must create subclasses per specific parent.

Essentially there are some options to workaround this:

  1. Wait until Apple and Google add lifecycle events. That way we can create our own listeners or delegates.
  2. Create shared managers/utilties which we then delegate the logic to with 1 single call in 1 subclass per parent type.
  3. Swallow our pride and create a whole chain of subclasses per root type and per functionality. This becomes nasty very quickly.
  4. Change the compiled or byte code of the top most parent type. By adding lifecycle events we are essentially doing point 1 ourselves.

From how above list is formatted and the title of the article you can guess my preference.

**Intermezzo: RxSwift**  
Since I use Rx (Reactive Extensions) a lot I found I had the following pattern quite often:

<pre class="brush: plain; title: ; notranslate" title="">var disposable: Disposable? = nil
func viewDidLoad() {
  disposable = getSomeNiceObservable()
    .map(someTransformation)
    .subscribeNext { print($0) }
}
func viewDidUnload() {
  disposable?.dispose()
}
</pre>

The same holds for Android. Luckily RxSwift adds a `rx_deallocated` Observable to NSObject which emits a value and completes in `deinit`. Now above code could be written like this:

<pre class="brush: plain; title: ; notranslate" title="">func viewDidLoad() {
  _ = getSomeNiceObservable()
    .map(someTransformation)
    .takeUntil(rx_deallocated)
    .subscribeNext { print($0) }
}
</pre>

Much shorter, and no extra instance variables needed, Eureka!

But this only works for deallocation, and as we are humans, we leak memory. If we retain self in the Observable sequence (this is quite common to accidentally do) a retain cycle is created between the Observable and self, the NSObject. To help and not hinder the programmer, it would be much easier if we could cleanup if the view is dismissed!

**Introducing RxLifecycle**  
I created [RxLifecycle][1], which works much like how RxSwift creates `rx_deallocated`. It provides 6 new Observables. Using a process called _swizzling_ we swap the methods that correspond to the lifecycle with our own methods which trigger a Rx.Subject which is exposed by a normal field in a Swift extension. This way two `Observable<Bool>` are exposed:

  * rx_visible: did the controller appear/disappear?
  * rx_willBeVisible: will the controller appear/disappear?

Those are then conveniently split into `Observable<Void>` per distinct event:

  * rx_willAppear
  * rx_willDisappear
  * rx_appear
  * rx_disappear

Example usage:

<pre class="brush: plain; title: ; notranslate" title="">Observable.interval(10, scheduler: MainScheduler.instance)
  .takeUntil(self.rx_disappear)
  .subscribeNext { print("\($0)") }
</pre>

Find the full source at <a href="https://gist.github.com/hermanbanken/4ac458aede6290b59a4acd5d52b81a6f" target="_blank">https://gist.github.com/hermanbanken/4ac458aede6290b59a4acd5d52b81a6f</a>.

 [1]: https://gist.github.com/hermanbanken/4ac458aede6290b59a4acd5d52b81a6f