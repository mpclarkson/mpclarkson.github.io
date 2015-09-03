---
layout: post
title: How to Draw a Speech Bubble in Swift 2.0
comments: true
---

Here's a quick example of how to create a custom UIView to display a speech bubble in Swift 2.0.

## Step 1: Subclass UIView

Firstly, you need to subclass `UIView` as follows.

If you intended to use *storyboard*, ensure that you replace the `fatalError` line with `super.init(coder: aDecoder)`

{% highlight swift %}

class SpeechBubble: UIView {

    var color:UIColor = UIColor.grayColor()

    override init(frame: CGRect) {
        super.init(frame: frame)
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

{% endhighlight %}

<!--more-->

## Step 2: Override `drawRect(rect: CGRect)`

Do *not* call `super.drawRect(rect)` in this method!

{% highlight swift %}

  override func drawRect(rect: CGRect) {

    let rounding:CGFloat = rect.width * 0.02

    //Draw the main frame
    let bubbleFrame = CGRect(x: 0, y: 0, width: rect.width, height: rect.height * 2 / 3)
    let bubblePath = UIBezierPath(roundedRect: bubbleFrame, byRoundingCorners: UIRectCorner.AllCorners, cornerRadii: CGSize(width: rounding, height: rounding))

    //Color the bubbleFrame
    color.setStroke()
    color.setFill()
    bubblePath.stroke()
    bubblePath.fill()

    //Add the point
    let context = UIGraphicsGetCurrentContext()

    //Start the line
    CGContextBeginPath(context)
    CGContextMoveToPoint(context, CGRectGetMinX(bubbleFrame) + bubbleFrame.width * 1/3, CGRectGetMaxY(bubbleFrame))

    //Draw a rounded point
    CGContextAddArcToPoint(context, CGRectGetMaxX(rect) * 1/3, CGRectGetMaxY(rect), CGRectGetMaxX(bubbleFrame), CGRectGetMinY(bubbleFrame), rounding)

    //Close the line
    CGContextAddLineToPoint(context, CGRectGetMinX(bubbleFrame) + bubbleFrame.width * 2/3, CGRectGetMaxY(bubbleFrame))
    CGContextClosePath(context)

    //fill the color
    CGContextSetFillColorWithColor(context, color.CGColor)
    CGContextFillPath(context);

    }
 }

{% endhighlight %}

## Step 3: Add a convenience initializer

Now we just need to add a convenience initializer so we can change the default color if required.

{% highlight swift %}

 required convenience init(withColor frame: CGRect, color:UIColor? = .None) {
    self.init(frame: frame)

    if let color = color {
        self.color = color
    }
 }

{% endhighlight %}

You can now instantiate a speech bubble with a color as follows:

{% highlight swift %} let speechBubble = SpeechBubble(withColor: CGRect(x: 0, y: 0, width: 200, height: 200), color: .redColor()) {% endhighlight %}

Of course, you can easily tweak this class to make it more flexible, such as customizing how rounded the edges are or adding a `UILabel` to display the number of comments etc.

Here's the full class for reference:

{% highlight swift %}

class SpeechBubble: UIView {

    var color:UIColor = UIColor.grayColor()

    override init(frame: CGRect) {
        super.init(frame: frame)
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    required convenience init(withColor frame: CGRect, color:UIColor? = .None) {
        self.init(frame: frame)

        if let color = color {
            self.color = color
        }
    }

    override func drawRect(rect: CGRect) {

        let rounding:CGFloat = rect.width * 0.02

        //Draw the main frame
        let bubbleFrame = CGRect(x: 0, y: 0, width: rect.width, height: rect.height * 2 / 3)
        let bubblePath = UIBezierPath(roundedRect: bubbleFrame, byRoundingCorners: UIRectCorner.AllCorners, cornerRadii: CGSize(width: rounding, height: rounding))

        //Color the bubbleFrame
        color.setStroke()
        color.setFill()
        bubblePath.stroke()
        bubblePath.fill()

        //Add the point
        let context = UIGraphicsGetCurrentContext()

        //Start the line
        CGContextBeginPath(context)
        CGContextMoveToPoint(context, CGRectGetMinX(bubbleFrame) + bubbleFrame.width * 1/3, CGRectGetMaxY(bubbleFrame))

        //Draw a rounded point
        CGContextAddArcToPoint(context, CGRectGetMaxX(rect) * 1/3, CGRectGetMaxY(rect), CGRectGetMaxX(bubbleFrame), CGRectGetMinY(bubbleFrame), rounding)

        //Close the line
        CGContextAddLineToPoint(context, CGRectGetMinX(bubbleFrame) + bubbleFrame.width * 2/3, CGRectGetMaxY(bubbleFrame))
        CGContextClosePath(context)

        //fill the color
        CGContextSetFillColorWithColor(context, color.CGColor)
        CGContextFillPath(context);
    }
}

{% endhighlight %}

Happy coding.