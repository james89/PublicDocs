---
layout: article
title: Migrating to Rufus 2.0
attribution: 
resource: true
categories: [Resources]
---


## What is Rufus 2.0?

Rufus is the codename for the new front-end widget framework, which utilizes our API + a ton of optimizations explained below.

## What you need to know

Migration to the latest version of our widget structure is something we are offering as a complimentary service to our existing partners.

The reason that we are doing this is because we have made a number of improvements , and additional features that requires us to do this as a larger process, rather than just a small patch update.

That being said, we have developed a migration plan so that there will be zero downtime to your existing widget implementations, no change in work flow for your moderators, and minimal development resources on your end.

## What we need from you

+ Access to a staging environment where we can preview the new widgets.
+ Contact with a developer that can copy and paste some code snippets

## The Benefits

+ Access to our new <strong>E</strong>nhanced <strong>M</strong>ach<strong>i</strong>ne <strong>L</strong>earning <strong>I</strong>mage <strong>O</strong>ptimization algorithm (EMiLIO) that ranks photos based on 39 different parameters, allowing you to surface the highest converting photos at the top of your widgets.
+ Faster load times on widgets.
+ New sorting methods, including: 
  + ***Photorank*** - Sort photos using Olapic machine-learning algorithm, EMiLIO (See above)
  + ***Random*** - Sort pictures randomly (gets reset once per day) 
  + ***Click-through rate*** - Sort pictures by click through rate while prioritizing new content
+ Category- and stream-level filtering of images
+ Cleaner, more robust responsive design that is built on the Bootstrap responsive framework.
+ Widget Instances allowing you to change the way widgets are populated without having to generate new JavaScript codes.
+ New features which improve UX, including: a grid and list view for wall widgets, a new sticky menu, and a return to top button out of the box.
+ New and improved Dev Tools, such as:
  + ***Development Mode*** - When enabled, all changes to the templates will bypass cache and take place immediately, allowing for faster development.
  + ***New JS Callbacks*** - All Olapic templates and widgets now include their own JS Callbacks.
  + ***{{debug}} mode*** - Debug any object within the template or widget structure simply by placing {{debug}} into the HTML.

## The Process For You

1. We hand over new widget instance codes to your developer with instructions to be copy and pasted into your staging environment *.
2. After these are implemented we begin development, which will typically take about **five business days** **.
3. After a thorough QA on our side, you can now view the widgets and make sure they meet the expected specification.
4. Push the staging widgets to your live environment.

\* ***Note:*** If you are currently using a CSV as a product feed we will need to upgrade you to an XML. This preferably will be in our official Olapic schema found here: [http://olapic.github.io/PublicDocs/product-feed.html](). If this particular schema can't be achieved we can do a custom feed, however it still must be XML format and we cannot guarantee the same consistency of import when done in this manner.</em>

\*\* ***Note:*** This can take longer if there are any custom features or special concerns to take into account with your e-commerce environment. We will let you know if this is the case.</em>
