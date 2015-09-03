---
layout: post
title: Insert Horizontal Separators Between UITabBarItems in Swift 2.0
comments: true
---

Here's a quick overview of how to insert horizontal separators between each `UITabBarItem` in a `UITabBarController` in Swift 2.0 just like this:

![swift uitabbaritem horizontal separator](/assets/tab-bar-separator.png)

All the necessary code goes in the `viewDidLoad` method of your `UITabBarController`.

<!--more-->

{% highlight swift %}


override func viewDidLoad() {
   super.viewDidLoad()

   if let items = self.tabBar.items {

       //Get the height of the tab bar
       let height = CGRectGetHeight(self.tabBar.bounds)

       //Calculate the size of the items
       let numItems = CGFloat(items.count)
       let itemSize = CGSize(
                        width: tabBar.frame.width / numItems,
                        height: tabBar.frame.height)

       for (index, _) in items.enumerate() {

           //We don't want a separator on the left of the first item.
           if index > 0 {

               //Xposition of the item
               let xPosition = itemSize.width * CGFloat(index)

               /* Create UI view at the Xposition,
                  with a width of 0.5 and height equal
                  to the tab bar height, and give the
                  view a background color
               */
               let separator = UIView(frame: CGRect(
                    x: xPosition, y: 0, width: 0.5, height: height))
               separator.backgroundColor = UIColor.grayColor()
               tabBar.insertSubview(separator, atIndex: 1)
           }
       }
   }
}

{% endhighlight %}

And.... there we go. You now have a nice thin horizontal separator between each tab bar item.

Code on!