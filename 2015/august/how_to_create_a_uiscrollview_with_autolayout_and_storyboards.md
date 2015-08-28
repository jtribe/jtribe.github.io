# How to create a UIScrollView with Auto Layout and Storyboards w/ Swift 2.0 | Part 1: Static Views

I've just recently started to learn Auto Layout after deciding that I had put it off for too long.  

I wanted to make a horizontal UIScrollView containing buttons, but I found it quite difficult to find a good tutorial online for using a UIScrollView with Auto Layout.  

In the end, I discovered two ways to make a UIScrollView with Auto Layout and one special edge case:

1. Where you know before compile time how many views you have within your UIScrollView (this is the easiest way)
2. You have a dynamic number of items within your UIScrollView to be created at run time
3. You have a dynamic number of buttons within your UIScrollView

I'm going to dedicate a blog post to each one of these scenarios and this is #1 in the series. So let's get started...


---

## UIScrollView with Static Views

### Step 1
First things first, add a UIScrollView to your main Storyboard

![Add UIScrollView To Storyboard Image](http://i.imgur.com/OY3RkqJ.png =800x)

### Step 2
- Make the UIScrollView the size of your Scene (it doesn't have to be, but for this example it is)  
- Click on the 'Pin' Constraints button (**|-[ ]-|** -> It looks like a Tie-Fighter)  in the bottom left hand corner  
- Add 4 constraints, pinning the UIScrollView to all sides of the top level view -> leading, top, trailing and bottom  
- Remember to deselect 'constrain to margins'.  This isn't essential, but it'll pin the UIScrollView relative to the top level view's edges instead of the layout guides

![Add Auto Layout Constraints to UIScrollView](http://i.imgur.com/nu0pS51.png =800x)

### Step 3
So here's where some of the magic happens.  Instead of adding your views directly to the UIScrollView, we're going to add a 'View' from the Object Library in the right hand toolbar to be our **Content View**.  This is going to make your life much much easier and i'll explain why shortly.  I've made our Content View green so it's easier to see.

![Adding Content View to UIScrollView](http://i.imgur.com/vWCGTw4.png =800x)

Again, as in step two, click on our Content View, then on the 'Pin' button and add 4 constraints for leading, top, trailing, bottom **and** height and width constraints.  Read that again, it's important.  Height **and** width constraint.  Add them.  Thanks.  This pins our Content View to all edges of our UIScrollView.

![Adding Content View Constraints](http://i.imgur.com/TuaXjCP.png)

### How UIScrollView works with Auto Layout
Now seems like a good time to explain how UIScrollView and Auto Layout work together.  Basically, UIScrollView works differently than how a normal view typically behaves when it comes to Auto Layout.  

#### UIScrollView's visible Rect Vs. Content View's Rect
When we pinned the UIScrollView to our top level view's edges in Step 2, we're saying "Our UIScrollView's **visible area** should pinned directly to our top level view's edges" but these are **not** our content view's constraints (read: what's inside our UIScrollView).

So you're probably thinking "Wait, if we've pinned our Content View's edges in Step 3 to our UIScrollView's edges, doesn't that mean that our Content View will always be the same size as our UIScrollView and never actually scroll?"  Astutely noticed good sir (or madam), but actually because our Content View has a constraint that says it's 600px wide, and when we run our app on the simulator the UIScrollView will only be the width of our screen (which is < 600px) the UIScrollView will be able to scroll! 

#### How does Auto Layout know how big my contentSize is?

Auto Layout will figure out the width and height of our Content View as long as all sides of our Content View are constrained to the edges of our UIScrollView **and** we have width and height constraints defined for our Content View.  

If you ever get the error message: *"UIScrollView has ambiguous scrollable content"* this is because you haven't constrained all sides of your Content View to your UIScrollView and defined width and height constraints for your Content View i.e. if this is not done, Auto Layout literally cannot figure out how big your UIScrollView should be because it doesn't know the size of it's content.  Makes sense, right?

So we need to make sure that our Content View's height or width is actually **bigger** than our UIScrollView's *at runtime*.  This tells the UIScrollView and Auto Layout *"Hey, you're as wide and tall as the screen you're on, but your content view is actually bigger than that, so i'll let you scroll to be able to see the rest of the content."*

Because we want to make a horizonatally scrolling UIScrollView, we'll have to adjust the Content View height constraint in the code, but more on that later.

### Step 4

#### ViewController Simulated Size: Fixed Vs. Freeform
For this tutorial, we're focusing on static views.  Our ViewController's view (top level view) is currently 600x600 as we're using the [SizeClass wAny hAny.](http://www.raywenderlich.com/83276/beginning-adaptive-layout-tutorial)  

1. Let's make our ViewController's size 'Freeform', by clicking on the yellow 'ViewController' icon on top of our view or in the right hand 'Document Outline' toolbar.  
![ViewController Button on View](http://i.imgur.com/bcmxE5i.png) ![ViewController Button on Document Outline](http://i.imgur.com/bH4oITF.png)

2. Go to the right hand toolbar, and click on the Size Inspector Icon (it looks like a ruler), and select 'Freeform' from the Simulated Size drop down.  
![Simulated Size Dropdown](http://i.imgur.com/WzSRKex.png)
3. Now we can resize our view to be any size we want.  Let's keep the height 600px, but make the width 1200px.  We're going to make a horizontally scrolling UIScrollView.
4. Click on the top level view in the Document Outline window on the left hand side, and then adjust it's width using the Size Inspector (it's the same ruler icon as before) to be 1200px

### Step 5

You'll notice that we now get Auto Layout errors, that's because we added a contraint for our UIScrollView to always be pinning to the leading (right hand) edge of the top level view, and now it's not because the top level view is 1200px long and our UIScrollView is only 600px!  Don't worry, it's an easy fix.  In fact, Xcode will do it for you if you click on the warning triangle, select 'Update Frame' and click the 'Fix Misplacement' button.  Phew!
![Fixing Constraints Automatically in Xcode](http://i.imgur.com/CvVwkvW.png)

Now our Content View is only 600px wide because added a 600px width constraint earlier. Let's do the same thing for our Content View and make it 1200px wide:

1. Click on our Content View and then click the Size Inspector (ruler icon) again and modify the width constraint to be 1200px.  If your 'trailing' constraint is not 0, set it to 0.
![Fixing Constraints Manually](http://i.imgur.com/mxTJMUm.png)

### Step 6

It's important to note now, that our UIScrollView will already scroll, and that as soon as we had set the constraints on the Content View, it would scroll because Auto Layout could figure out that our Content View was larger in both Height and Width than our UIScrollView at run time.  Don't believe me?  Try it out!  Run the app on the simulator and you'll see that you can scroll in all directions.

### Step 7

But we don't want it to scroll vertically, and our Content View is a bit boring, so lets start by adding some views to our content view.

1. Grab a View object from the Object Library and add it to our content view
2. Lets set leading, top, bottom and width:300 constraints on our new view -> This sets our view to be pinned to the left, top and bottom edges of the Content View and sets it's width to be 300px  
![Setting our new View's Constraints with Auto Layout](http://i.imgur.com/FG6KPsV.png)

### Step 8

1. Copy and paste our new view 3 more times and lets give them all different colours
2. So for each view:
	* View 1: x: 0, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 1's leading constraint will be to the Content View
	* View 2: x: 300, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 2's leading constraint will be to View 1
	* View 3: x: 600, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 3's leading constraint will be to View 2
	* View 4: x: 900, y: 0, w: 300, h: 600 -> where width is also a constraint and height is not.  View 4's leading constraint will be to View 3
3. We'll also need to add a trailing constraint for View 4 -> Content View.  In the end you should have something like this:
![Finished Content View within UIScrollView](http://i.imgur.com/gYRTZJT.png =800x)


### Step 9

If you ran the app again now, you'd see that we have all of our views scrolling nicely, but we're still able to scroll vertically!  Arrghhhhh.  Let's fix that right now!

1. Click on the Content View's height constraint in the Document Outline toolbar on the left hand side.  
![Content View Height Constraint in Document Outline](http://i.imgur.com/YzZaXJT.png)

2. Control + Drag from the constraint to our .swift file and name our constraint Content ViewHeightConstraint
![Control + Drag Constraint From Storyboard](http://i.imgur.com/VBcgSbV.png)
![What the Auto Layout constraint looks like when it's finished](http://i.imgur.com/SCKwCgu.png)

3. Finally, we need to implement and override viewDidLayoutSubviews and set our Content ViewHeightConstraint to be equal to the height of our top level view (which, in this case, is the height of the screen).  viewDidLayoutSubviews is called to notify our controller that our view has just laid out our subviews, it's usually called multiple times during the creation of our view controller so we can be sure that our Content View gets set to the appropriate height when Auto Layout figures out the height of our top level view (read: screen height)
![Updating our Constraint in viewDidLayoutSubviews](http://i.imgur.com/Wyh1xjK.png)

### Step 10: Voila

There is no Step 10.  Our horizontal UIScrollView scrolls beautifully.

![UIScrollView with Auto Layout and Storyboard GIF](http://i.imgur.com/l7rgfe9.gif)