# How to create a UIScrollView with Autolayout and Storyboards w/ Swift 2.0

There are two ways to make a UIScrollView with Autolayout

1. Where you know before compile time how many views you have within your scrollView (this is actually easier)
2. You have a dynamic number of items within your scrollView to be created at run time

---

## UIScrollView with Static Views

### Step 1
First things first, add a UIScrollView to your main Storyboard

![Add UIScrollView To Storyboard Image](http://i.imgur.com/OY3RkqJ.png =800x)

### Step 2
- Make the scrollView the size of your Storyboard (it doesn't have to be, but for this example it is)  
- Click on the 'Pin' Constraints button (**|-[ ]-|** -> It looks like a Tie-Fighter)  in the bottom left hand corner  
- Add 4 constraints, pinning the scrollView to all sides of the top level view -> leading, top, trailing and bottom  
- Remember to deselect 'constrain to margins'.  This isn't essential, but it'll pin the scrollView relative to the top level view's edges instead of the layout guides

![Add Autolayout Constraints to UIScrollView](http://i.imgur.com/nu0pS51.png =800x)

### Step 3
So here's where some of the magic happens.  Instead of adding your views directly to the scrollView, we're going to add a 'View' from the Object Library in the right hand toolbar to be our **Content View**.  This is going to make your life much much easier and i'll explain why shortly.  I've made our contentView green so it's easier to see.

![Adding ContentView to UIScrollView](http://i.imgur.com/vWCGTw4.png =800x)

Again, as in step two, click on our contentView, then on the 'Pin' button and add 4 constraints for leading, top, trailing, bottom and for our contentView, we need to add height and width constraints.  This pins our contentView to all edges of our scrollView.

![Adding ContentView Constraints](http://i.imgur.com/TuaXjCP.png)

### How UIScrollView works with Autolayout
Now seems like a good time to explain how UIScrollView and Autolayout work together.  Basically, UIScrollView works differently than how a normal view typically behaves when it comes to Autolayout.  

#### ScrollView's visible Rect Vs. ContentView's Rect
When we pinned the scrollView to our top level view's edges in Step 2, we're saying "Our scrollView's **visible area** should pinned directly to our top level view's edges" but these are **not** our content view's constraints (read: what's inside our scrollView).

So you're probably thinking "Wait, if we've pinned our contentView's edges in Step 3 to our scrollView's edges, doesn't that mean that our contentView will always be the same size as our scrollView and never actually scroll?"  Astutely noticed good sir (or madam), but actually because our contentView has a constraint that says it's 600 pixels wide, and when we run our app on the simulator the scrollView will only be the width of our screen (which is < 600) the scrollView will be able to scroll! 

#### How does Autolayout know how big my contentSize is?

Autolayout will figure out the width and height of our contentView as long as all sides of our contentView are constrained to the edges of our scrollView **and** we have width and height constraints defined for our contentView.  

If you ever get the error message: *"ScrollView has ambiguous scrollable content"* this is because you haven't constrained all sides of your contentView to your scrollView and defined width and height constraints for your contentView i.e. if this is not done, autolayout literally cannot figure out how big your scrollView should be because it doesn't know the size of it's content.  Makes sense, right?

So we need to make sure that our contentView's height or width is actually **bigger** than our scrolView's.  This tells the UIScrollView and Autolayout *"Hey, you're as wide and tall as the screen you're on, but your content view is actually bigger than that, so i'll let you scroll to be able to see the rest of the content."*

Because we want to make a horizonatally scrolling scollView, we'll have to adjust the contentView height constraint in the code, but more on that later.

### Step 4

#### ViewController Simulated Size: Fixed Vs. Freeform
For this tutorial, we're focusing on static views.  Our ViewController's view (top level view) is currently 600x600 as we're using the SizeClass wAny hAny.  

1. Let's make our ViewController's size 'Freeform', by clicking on the yellow 'ViewController' icon on top of our view or in the right hand 'Document Outline' toolbar.  
![ViewController Button on View](http://i.imgur.com/bcmxE5i.png) ![ViewController Button on Document Outline](http://i.imgur.com/bH4oITF.png)

2. Go to the right hand toolbar, and click on the Size Inspector Icon (it looks like a ruler), and select 'Freeform' from the Simulated Size drop down.  
![Simulated Size Dropdown](http://i.imgur.com/WzSRKex.png)
3. Now we can resize our view to be any size we want.  Let's keep the height 600, but make the width 1200.  We're going to make a horizontally scrolling UIScrollView.
4. Click on the top level view in the Document Outline window on the left hand side, and then adjust it's width using the Size Inspector (it's the same ruler icon as before) to be 1200

### Step 5

You'll notice that we now get AutoLayout errors, that's because we added a contraint for our scrollView to always be pinning to the leading (right hand) edge of the top level view, and now it's not because the top level view is 1200 pixels long and our scrollView is only 600!  Don't worry, it's an easy fix.  In fact, XCode will do it for you if you click on the warning triangle, select 'Update Frame' and click the 'Fix Misplacement' button.  Phew!
![Fixing Constraints Automatically in XCode](http://i.imgur.com/CvVwkvW.png)

Now our contentView is only 600 pixels wide because we told it to be via a width constraint earlier. Let's do the same thing for our contentview.

1. Click on our contentView and then Click the Size Inspector (ruler icon) again and modify the width constraint to be 1200.  If your 'trailing' constraint is not 0, set it to 0.
![Fixing Constraints Manually](http://i.imgur.com/mxTJMUm.png)

### Step 6

It's important to note now, that our scrollView will already scroll, and that as soon as we had set the constraints on the contentView, it would scroll because AutoLayout could figure out that our contentView was larger in both Height and Width than our scrollView at run time.  Don't believe me?  Try it out!  Run the app on the simulator and you'll see that you can scroll in all directions.

### Step 7

But we don't want it to scroll vertically, and our contentView is a bit boring, so lets start by adding some views to our content view.

1. Grab a View object from the Object Library and add it to our content view
2. Lets set leading, top, bottom and width:300 constraints on our new view -> This sets our view to be pinned to the left, top and bottom edges of the contentView and sets it's width to be 300px  
![Setting our new View's Constraints with AutoLayout](http://i.imgur.com/FG6KPsV.png)

### Step 8

1. Copy and paste our new view 3 more times and lets give them all different colours
2. So for each view:
	* View 1: x: 0, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 1's leading constraint will be to the contentView
	* View 2: x: 300, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 2's leading constraint will be to View 1
	* View 3: x: 600, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 3's leading constraint will be to View 2
	* View 4: x: 900, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 4's leading constraint will be to View 3
3. We'll also need to add a trailing constraint for View 4 -> contentView.  In the end you should have something like this:
![Finished ContentView within UIScrollView](http://i.imgur.com/gYRTZJT.png =800x)


### Step 9

If you ran the app again now, you'd see that we have all of our views scrolling nicely, but we're still able to scroll vertically!  Arrghhhhh.  Let's fix that right now!

1. Click on the contentView's height constraint in the Document Outline toolbar on the left hand side.  
![Content View Height Constraint in Document Outline](http://i.imgur.com/YzZaXJT.png)

2. Control + Drag from the constraint to our .swift file and name our constraint contentViewHeightConstraint
![Control + Drag Constraint From Storyboard](http://i.imgur.com/VBcgSbV.png)
![What the AutoLayout constraint looks like when it's finished](http://i.imgur.com/SCKwCgu.png)

3. Finally, we need to implement and override viewDidLayoutSubviews and set our contentViewHeightConstraint to be equal to the height of our top level view (which, in this case, is the height of the screen).  viewDidLayoutSubviews is called to notify our controller that our view has just laid out our subviews, it's usually called multiple times during the creation of our view controller so we can be sure that our contentView gets set to the appropriate height when AutoLayout figures out the height of our top level view (read: screen height)
![Updating our Constraint in viewDidLayoutSubviews](http://i.imgur.com/Wyh1xjK.png)

### Step 10: Voila

There is no Step 10.  Our horizontal scrollView scrolls beautifully.

## Coming to a screen near you soon....  

### How to set up a UIScrollView with AutoLayout and Storyboards and dynamically created UIViews.