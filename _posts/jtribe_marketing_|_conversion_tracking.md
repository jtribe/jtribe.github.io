# jtribe Marketing

# Part 1 - Conversion Tracking

## Why do we need conversion tracking in marketing?
We need to be able to measure the effectiveness of our marketing efforts so that we can concentrate on profitable streams and quickly disregard streams that are not working.  We need to know that if we put $X into a specific stream we get $Y out with the ultimate goal if our ratio of $in to $out being > 1.

## What is a conversion?
I would say that a `conversion` for jtribe - in terms of web - should be defined as someone who has used the ‘Contact Us’ to lodge an inquiry regarding app development.  

A `conversion` could also be defined via an email and phone call and should be taken into consideration when calculating the effectiveness of a marketing stream, depending on the stream of course:

### Scenario 1
If someone calls us we should always ask them where they found our number and ultimately, what were the set of actions that led to the phone call.  If they found the number on our website via clicking ‘Contact Us’ and they got to the website via a Google ad then that’s valuable information and should be tracked accordingly!

### Scenario 2
If we have an experiment that is handing out leaflets (or business cards) at an expo, we should get a cheap simcard and use the new phone number for the leaflets, and see how many people call that number.  This would allow us to accurately measure the effectiveness of that experiment.

## Conversion funnels
>A Conversion funnel is a phrase used in e-commerce to describe the journey a consumer takes through an Internet advertising or search system, navigating an e-commerce website and finally converting to a sale. The metaphor of a funnel is used to describe the decrease in numbers that occurs at each step of the process.

Now that we have a `conversion` defined i.e. someone who has used the ‘Contact Us’ to lodge an inquiry regarding app development, we can begin to look at how we can create our conversion funnel.  Here is a basic example of a conversion funnel:

```
Potential customer clicks on Google ad → Goes to landing page → Clicks ‘Contact Us’ → Fills in form → clicks ‘Send’ button.
```

It is imperative that we are able to accurately track the effectiveness of our conversion funnel at each stage, so how do we do that?

## Special Links | utm links
In a nutshell, utm links are special links that allow you to track where your potential customers came from when they clicked a link that brought them to your landing page.  

Through utm links we can define a variety of things we wish to track:

1. **Source** | `utm_source` | - The referrer e.g. Google Ad, Facebook, Newsletter, Twitter etc.
2. **Medium** | `utm_medium` | - type of advert e.g. banner ad on site, ‘cost-per-click’ ad on social media, email, retargeting ad
3. **Term** | `utm_term` | - search terms / the keywords used 
4. **Content** | `utm_content` | - used to differentiate ads e.g. title text on advert 
5. **Name** | `utm_campaign` | - product name, slogan, promo code etc.

Here’s an example link:

```
www.jtribe.com.au/?utm_source=google&utm_medium=cpc&utm_term=love%20your%20apps&utm_campaign=app%20development
```

Let’s break it down:

1. www.jtribe.com.au/
2. ?utm_source=google
3. &utm_medium=cpc
4. &utm_term=love%20your%20apps
5. &utm_campaign=app%20development

Through looking at this link I can instantly tell that it’s from a **google ad (2)**, it’s a **cost-per-click advert (3)** and the title we used on the advert was **“Love your apps” (4)**

The easiest way to create a utm link is to use this [simple utm link builder](https://support.google.com/analytics/answer/1033867?hl=en)

## Capturing utm link data
We’ve got our special link, we’ve set it as the link when a user clicks a particular advert, so how do we actually capture that data on our webpage so that all of this effort isn’t wasted? Well, there are a few different ways depending on what tools we’re using, but they all follow the same basic principle:
 
```
We use the `<form>` tag in html with hidden fields named: `utm_source`, `utm_medium`, `utm_term` and `utm_campaign
```

### Manual Capture
If we were to capture this manually, we’d need to write js to parse the url, capture the url parameters, insert them into the hidden fields and then assumedly we'd store this data somewhere for future analysis.


### Analytics Tracking Software

Alternatively, we can using analytics software such as [google analytics](https://www.google.com.au/analytics), [kissmetrics](www.kissmetrics.com) or [mixpanel](www.mixpanel.com) which will have built-in utm tracking available through their api.  This has a few advantages:

* Usually later down the line we'll want analytics tracking software anyway to accurately track each stage of our conversion funnel, so we might as well use it now
* We don't have to write our own code
* We don't have to store the data locally 
* We don't have to build or acquire our own tools for interpreting the captured data
* Analytics software usually have really great data visualisation tools and conversion funnel creation tools that come for free by using the software

## Building A Conversion Funnel

Essentially to create a conversion funnel we have to identify key steps for a potential customer to become an acquired customer and use analytics software to track this appropriately. 

A major upside to using existing analytics software is that it allows us to rapidly create and visualise a series of events (or touch points) as a conversion funnel and more importantly, it allows us to instantly see where our conversion funnel may be failing.  

Here is a conversion funnel for a sock company:

```
→ Clicks on Facebook ad for Red socks / Goes to landing page 
→ Clicks ‘Buy Now’ 
→ Stage 1 | Fills in Personal Details
→ Stage 2 | Fills in Payment Details
→ Stage 3 | Payment Confirmation Checkout Process
```

We have 5 important touch points.  We can track each touch point by using analytics software to send an event on each button click:

1. **Clicks on Facebook ad for Red socks / Goes to landing page** - Facebook will show us the % of users that clicked on our ads and what % of our users 'bounced' i.e. Bounce rate is the % of visitors who came to our website and leave without viewing any other pages
2. **Clicks ‘Buy Now’** - we can use software like mixpanel to track how many people that visited this particular web page clicked the 'buy now' button i.e. send an event on click via mixpanel api
3. **Stage 1 | Fills in Personal Details** - Again, use mixpanel or otherwise to send an event when the user successfully completes this part of the checkout process.  It's important to note that we can send additional data with each event e.g. how many fields were completed when the user advanced to the next stage
4. **Stage 2 | Fills in Payment Details** - as above
5. **Stage 3 | Payment Confirmation Checkout Process** - as above

All of this data is really helpful.  For example, lets say that the % of people who advance to Stage 3 of the checkout process drops off significantly compared to the rest of the touch points in the funnel.  In this case, we'd instantly know that if we 'fix' our payment detail capture process we'd make huge gains!!! 

## Traffic Quality

As well as being able to see where our funnel fails, analytics and defining a solid convesion funnel also allows us to check the quality of the traffic coming through our funnel.  In the example of the sock company again: 

```
→ We have two ads running on Facebook, one for red socks and one for blue socks
→ 12% of people who see the red socks ad click through to our website and only 6% of people who see the blue sock image click through

```

So far, this looks pretty bad for the blue socks, if we had stopped here and not created a full conversion funnel covering all major touch points, there's a high possibility that we would drop the blue socks ad and push twice as much money into the red socks ad.  Afterall, it's click-through rate is 100% higher! 

However, since we've covered all touch points, we can find the % of people that have came from red / blue socks and bought a pair of socks.  It could very well be that the people that arrived at the website via red socks ad had a conversion rate of 10% whereas the people who arrived via the blue socks ad had a conversion rate of 30%.

## Tracking Pixels

Some websites such as facebook allow you to paste a js script called a 'pixel' into your website, which will send information back to facebook at the correct time.  For example, when running facebook ads for the sock company we have added our facebook conversion pixel to the `<head>` of our 'Purchase Confirmation' page (which is the final step of our conversion funnel, this is where we know someone has made a purchase).  This allows us to do a simple conversion funnel

```
→ Potential customer clicks ad on Facebook
→ Potential customer buys a pair of socks
```
Now when we look at our Ads Manager on facebook it can tell us the % of people who clicked on the advert and the % of people who completed a purchase from the advert.  Of course, the biggest problem with this is that with only two touch points you can't really be sure where your customers dropped off in your conversion funnel... was it the first page?  The payment details page?  etc.