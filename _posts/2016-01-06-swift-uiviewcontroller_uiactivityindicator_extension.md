---
layout: post
title: UIViewController Extension to create a global UIActivityIndicatorView
comments: true
---

You shouldn't repeat code, so don't!

One of the many things iOS developers need to do over and over again is to display a `UIActivityIndicatorView` in a `UIViewController`.
However, most of the time this seems to be accomplished by adding a `UIActivityIndicatorView` to each controller, which does not adhere to the DRY
principle.

Thankfully, using Swift extensions, it is very easy to create a global activity indicator.

<!--more-->

Firstly create UIViewController extension with a publicly accessible computed `Int` property,
to use as the activity indicator `tag` identifier.

You can change the number to anything or override it in an individual controller,
just make sure no other view has the same tag!

{% highlight swift %}

extension UIViewController {

    var activityIndicatorTag: Int { return 999999 }
}

{% endhighlight %}

Now we create a method called `startActivityIndicator` that dynamically creates an activity indicator and adds
it to a view controller's view.

This implementation allows you to customize the style (default is `.Gray`)
and the location (default is the view controller `view.center`) but you can
easily modify this (e.g. To disable touches).

{% highlight swift %}

extension UIViewController {

    //Previous code

    func startActivityIndicator(
        style: UIActivityIndicatorViewStyle = .Gray,
        location: CGPoint? = nil) {

        //Set the position - defaults to `center` if no`location`
        //argument is provided
        let loc = location ?? self.view.center

        //Ensure the UI is updated from the main thread
        //in case this method is called from a closure
        dispatch_async(dispatch_get_main_queue(), {

            //Create the activity indicator
            let activityIndicator = UIActivityIndicatorView(activityIndicatorStyle: style)
            //Add the tag so we can find the view in order to remove it later
            activityIndicator.tag = self.activityIndicatorTag
            //Set the location
            activityIndicator.center = loc
            activityIndicator.hidesWhenStopped = true
            //Start animating and add the view
            activityIndicator.startAnimating()
            self.view.addSubview(activityIndicator)
        })
    }
}
{% endhighlight %}

Now we create a method called `stopActivityIndicator` to remove the activity indicator.

{% highlight swift %}

extension UIViewController {

    //Previous code

     func stopActivityIndicator() {

        //Again, we need to ensure the UI is updated from the main thread!
        dispatch_async(dispatch_get_main_queue(), {
            //Here we find the `UIActivityIndicatorView` and remove it from the view
            if let activityIndicator = self.view.subviews.filter(
            { $0.tag == self.activityIndicatorTag}).first as? UIActivityIndicatorView {
                activityIndicator.stopAnimating()
                activityIndicator.removeFromSuperview()
            }
        })
    }
}

{% endhighlight %}

And there you have it - a globally accessible activity indicator that you can now call from any view controller!

This and many other extensions intended to reduce code repetition are available in the [HISwiftExtensions] (https://github.com/hilenium/HISwiftExtensions)library

Happy coding!
Matt