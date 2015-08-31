# How to create a UIScrollView with Auto Layout and Storyboards w/ Swift 2.0 | Part 2: Dynamic Views

For this blog post, i'm going to assume that you've already been through part 1 in our Auto Layout and UIScrollView series, focusing on static views **TO DO: here.**

This time, we're going to look at dynamic views.  What happens if you don't know how many views you will have at run time, and you want to add them in a pure auto layout way?  Lets find out...

---

## UIScrollView with Dynamic Views

This post starts with having our project file in the same state as it was in part 1, finishing step 6.  We've just added our Content View to our UIScrollView and added all constraints.  We're going to create the same UIScrollView as part 1, only this time all of the views will be done in code using Visual Format Language

### Step 1

In our last post, we only had a property for the Content View Height Contraint (`contentViewHeightConstraint`).  Let's add a property for our Content View Width Constraint and call it `contentViewWidthConstraint`  

![Adding Auto Layout Constraint](http://i.imgur.com/EHeESR7.png =800x)

### Step 2

Even though we're going to be creating all of our views in code, one of the things I like doing is creating a 'Template View' in a Scene.  This Template View helps us to not have 'Magic Numbers' lying around in our code, and by that I mean it gives us an example view to work from when creating our dynamic views in terms of height and width.  

Instead of saying our height is 600px and our width is 300px, we'd say that our height is equal to `CGRectGetHeight(templateView)` and `CGRectGetWidth(templateView)`.  Let's make a Template View in our scene: 

1. As in Step 7 in out last post, Grab a View object from the Object Library and add it to our content view
2. Lets set leading, top, bottom and width:300 constraints on our new view -> This sets our view to be pinned to the left, top and bottom edges of the Content View and sets it's width to be 300px.  It's important to note that our constraints here  are largely actually irrelevant.  They're just there to stop Xcode from complaining and it's good practice!  The only one we'll actually use is width.
3. Let's create a property from our Template View and call it `templateView`  

![Adding Template View](http://i.imgur.com/PAwBAuS.png =800x)


### Step 3

Now that we have our `templateView`, lets set the our constraints for the Content View and `templateView` so that their heights will be equal to the screen height that they're displayed on:

	override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        let screenHeight = CGRectGetHeight(view.frame)
        contentViewHeightConstraint.constant = screenHeight
        templateView.frame.size.height = screenHeight
    }
    
### Step 4

Alright we have our `templateView` height and width set correctly, now let's create and add our views.  The most important thing about adding UIViews to a UIScrollView using a pure Auto Layout approach is that we **do not set the frames or anything to do with the frames manually.** I repeat, **When using AutoLayout, you should never directly set the frame of a view.**  Auto Layout should take care of everything, and to do that we use Visial Formal Language, but more on that later.  Let's take some baby steps and create a function to add our views:

    func addViewsToScrollView() {
        let numberOfViews = 5
        contentViewWidthConstraint.constant = CGFloat(numberOfViews) * CGRectGetWidth(templateView.frame)
        for var index = 0; index < numberOfViews ; index++ {
            let view = UIView()
            view.translatesAutoresizingMaskIntoConstraints = false
            view.backgroundColor = getColor(index)
        }
    }
    
Firstly you'll notice that we haven't actually added the views to our Content View.  That's fine, we're not finished yet, in fact we don't even have a `contentView` property!  So let's add a property right now for our Content View and call it `contentView`.  You should have these 4 properties so far:

    @IBOutlet weak var contentViewHeightConstraint: NSLayoutConstraint!
    @IBOutlet weak var contentViewWidthConstraint: NSLayoutConstraint!
    @IBOutlet weak var templateView: UIView!
    @IBOutlet weak var contentView: UIView!
    
Ok?  Good.  Next thing you'll notice is this *huge* boolean property name `translatesAutoresizingMaskIntoConstraints`.  **When using pure Auto Layout to size and position our views we must set** `translatesAutoresizingMaskIntoConstraints` **to false.**  When a view's `translatesAutoresizingMaskIntoConstraints` is set to true, its super view uses the view's autoresizing mask to automatically create a set of strict constraints at run time, so it's pretty likely that if you add or change any constraints on the view that you're going to see a conflict of sorts.  

We don't want our super view to create any constraints for us, we don't want to touch the view's frame.  All we want to do is create the constraints ourselves in code using Visial Formal Language.  

Also, as a note, i've created a function called `getColor(index: int) -> UIColor` that takes the array index and returns a color:

    func getColor(index: Int) -> UIColor {
        switch index {
        case 0: return UIColor.redColor()
        case 1: return UIColor.greenColor()
        case 2: return UIColor.blueColor()
        case 3: return UIColor.yellowColor()
        case 4: return UIColor.orangeColor()
        default: return UIColor.clearColor()
        }
    }

### Step 5 - Visual Format Language

I think it's important to note that there are awesome libraries out there for creating Auto Layout constraints in code without having to touch Visual Format Language (VFL) e.g. [SnapKit](http://snapkit.io/).  I wanted to get down to the basics before moving on to using a library because if you've ever had any Auto Layout conflicts before, you know it can be quite handy to know VFL.  

If you know nothing about VFL then that's ok, you're like I was before I read [this awesome tutorial](http://code.tutsplus.com/tutorials/introduction-to-the-visual-format-language--cms-22715) and gained a solid understanding of it.  I recommend reading it before continuing any further.

![What Visual Format Language and Auto Layout Conflicts Looks Like](https://lh3.ggpht.com/MU-0Dw9Y9rzhujFZ3eJLhUzBm7DDUeKbd4X17uecWB8LZtLnEiAvp3cRdkrDHr-u8g=w300)  

*(What Visual Format Language and Auto Layout conflicts look like before reading the tutorial above)*

Now that we have a bit of an understanding about VFL, let's add constraints for our views in code.  Don't worry, i'll explain what the hieroglyphics-like sentences means.

### Step 6

    func addViewsToScrollView() {
        let numberOfViews = 5
        contentViewWidthConstraint.constant = CGFloat(numberOfViews) * CGRectGetWidth(templateView.frame)
        
        var views = [String: UIView] ()
        let metrics: [String: CGFloat] = [ "viewWidth" : CGRectGetWidth(templateView.frame), "viewHeight": CGRectGetHeight(templateView.frame) ]
        for var index = 0; index < numberOfViews ; index++ {
            let view = UIView()
            view.translatesAutoresizingMaskIntoConstraints = false
            view.backgroundColor = getColor(index)
            let viewName = "view\(index)"
            views[viewName] = view
        }
    }
    
Alright let's take this step by step.  If you read the above tutorial you'll know that when we create our constraints in code, we need a `Dictionary` of our views that we're applying the constraints to and a `Dictionary` of metrics, which helps keep our VFL Strings clean and easy to read e.g. viewWidth represents 300 instead of just writing 300 for our width.  *(aha! See how our* `templateView` *makes things a bit cleaner now?  Ignore the *`numberOfViews` *though, it's just a random number.).*

We've created two dictionaries called `views` and `metrics` respectively.  Each of our views need a name, so i've just called them `view0` through to `view4` with `viewName`.  We then assign the `viewName` to the view in our `views` dictionary.

### Step 7

    func addViewsToScrollView() {
        let numberOfViews = 5
        contentViewWidthConstraint.constant = CGFloat(numberOfViews) * CGRectGetWidth(templateView.frame)
        
        // Auto Layout Constraints Init
        var views = [String: UIView] ()
        let metrics: [String: CGFloat] = [ "viewWidth" : CGRectGetWidth(templateView.frame), "viewHeight": CGRectGetHeight(templateView.frame) ]
        var horizontalConstraintVFL = "H:|"  // 1
        
        for var index = 0; index < numberOfViews ; index++ {
            // Create a View
            let view = UIView()
            view.translatesAutoresizingMaskIntoConstraints = false
            view.backgroundColor = getColor(index)
            contentView.addSubview(view) // 2
            
            // Auto Layout Constraints
            let viewName = "view\(index)"
            views[viewName] = view
            horizontalConstraintVFL += "[\(viewName)(viewWidth)]" // 3
            horizontalConstraintVFL += (index < (numberOfViews - 1)) ? "-0-" : "|" // 4
        }
        
        // 5
        let horizontalConstraints = NSLayoutConstraint.constraintsWithVisualFormat(horizontalConstraintVFL, options: NSLayoutFormatOptions(rawValue: 0), metrics: metrics, views: views)
        contentView.addConstraints(horizontalConstraints)
    }
    
Don't worry about the big block of code, it's not as scary as it looks.  We've got 5 points in there to cover so we'll go through them 1 by 1  

1. We're initialising our `horizontalConstraintVFL` constraint to say "I'm a horizontal constraint `H:` and the first view added to me is going to have it's leading edge aligned to its super view's leading edge `|` without any spacing. 
2. Secondly, we actually add our views to the contentView
3. We're adding constraints for each view to say "I'm a view of width 300, defined by `viewWidth`" `[view0(viewWidth)]`
4. Then for each view other than the last view we add "I will have 0 spacing to the view next to me" `-0-` For the last view we do the same as the first view but only this time alight it to the trailing space of its superview `|`
5. We create our `horizontalConstraints` and add it to our `contentView` so that the Content View knows how to arrange its subviews.  Notice `NSLayoutFormatOptions(rawValue: 0)`, we're using no special formatting options here.

The final product when via `NSLog` should look like this:

	H:|[view0(viewWidth)]-0-[view1(viewWidth)]-0-[view2(viewWidth)]-0-[view3(viewWidth)]-0-[view4(viewWidth)]|
	
If you were to run the app right now, you wouldn't see our views, what's going on!?  Well, we haven't actually given our views any vertical constraints yet so they don't know how tall they should be!  Let's finish up by adding a Vertical Constraint for each `view` to our `contentView`

### Step 8 

Final Step! :)  Right after this line in our `for loop`:

	horizontalConstraintVFL += (index < (numberOfViews - 1)) ? "-0-" : "|" // 4

...and before the closing bracket of our loop add this:

    let verticalConstraint = NSLayoutConstraint.constraintsWithVisualFormat("V:|[\(viewName)]|", options: NSLayoutFormatOptions(rawValue: 0), metrics: metrics, views: views)
    contentView.addConstraints(verticalConstraint)
    
Here, we're adding a vertical constraint for each view so that the top and bottom edges of our view alights to the top and bottom edges of its superview without and spacing.  Nice and easy compared to the horizontal constraint.  

And that's it! Just add `addViewsToScrollView()` in your `viewDidLayoutSubviews()` and you're good to go!

    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        let screenHeight = CGRectGetHeight(view.frame)
        contentViewHeightConstraint.constant = screenHeight
        templateView.frame.size.height = screenHeight
        addViewsToScrollView()
    }        
    