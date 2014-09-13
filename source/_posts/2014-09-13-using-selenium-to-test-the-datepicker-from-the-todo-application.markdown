---
layout: post
title: "Using Selenium to test the AngularUI datepicker from the todo application"
date: 2014-09-13 16:32:56 +0200
comments: true
categories: scala play-framework angularui selenium specs2 FluentLenium
---

In the last post I added a date picker to my todo application which is built using Play framework in Scala and AngularJS.
Just to remember, here is a screenshot of the date picker :

{% img center /assets/posts_images/datepicker.png The date picker from AngularUI %}

What would be nice is to test it with Selenium. 
So the general goal of the test in my application is as follow :

{% img center /assets/posts_images/todo_selenium_test.png General goal of the test for the todo application %}

But what interest us here is this part :


{% img center /assets/posts_images/todo_selenium_test2.png The date picker part on the test %}

So the point of this part of the test is to be able to select the 9th of the current month from the date picker.

First step is to open the popup date picker, pretty simple I added an ID to the button for opening the date picker.
The corresponding code for the test just need to find the ID and click on the button (I'm using *`FluentLenium`*) :
{% highlight scala %}
browser.$("#date-picker").click()
{% endhighlight %}

Now the interesting part, in order to select the 9th we need to find a way to click on this day in the date picker.

The first thing to check is the html generated for the 9th of the month button.
{% highlight html %}
<td id="datepicker-008-6172-9" class="text-center ng-scope" aria-disabled="false" role="gridcell" ng-repeat="dt in row track by dt.date">
   <button class="btn btn-default btn-sm" tabindex="-1" ng-disabled="dt.disabled" ng-click="select(dt.date)" ng-class="{'btn-info': dt.selected, active: isActive(dt)}" style="width:100%;" type="button">
     <span class="ng-binding" ng-class="{'text-muted': dt.secondary, 'text-info': dt.current}">
       09
     </span>
   </button>
</td>
{% endhighlight %}

In this html, we need to find a unique pattern for selecting the 9th button.
As you can see, the *`td`* element has an ID. So we could use it directly.
Unfortunately, part of this ID is random but one part is always the same and correspond to the day of the month, so we can actually use this part. 
Indeed the last part of *`datepicker-008-6172-9`* correspond to the day, here it is *`9`*.
So we can use this as the pattern for selecting the day we want!

The following snippet contains the code for selecting the *`td`* element corresponding to the 9th day of the current month.
{% highlight scala %}
browser.$("td", withId().contains("-9")).find("button").click()
{% endhighlight %}

Explanation of the snippet :

In Play framework, *`$`* correspond to the *`find`* method in *`FluentLenium`* for searching element.
So basically, the snippet mean : Find a *`td`* element with an ID containing *`-9`*. Inside this element search for a *`button`* and click on it.

Here is what the test looks like with selenium in action :

{% img center /assets/posts_images/datepicker_selenium.gif General goal of the test for the todo application %}
