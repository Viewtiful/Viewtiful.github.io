---
layout: post
title: "Using AngularJS UI Datepicker with CoffeeScript"
date: 2014-09-12 10:55:48 +0200
comments: true
categories: scala play-framework coffeescript angularJS 
---

I'm currently playing around and building a simple todo application and
I wanted to add a date picker for the expected end date of a todo.

Good thing [AngularUI Bootstrap][AngularUI Bootstrap] proposes one!
Here is a screenshot

{% img center /assets/posts_images/datepicker.png The date picker from AngularUI %}

I chose to use it as a popup when clicking on a button at the right of the input element.
So I will explain how to use it this way. But there is many different ways to use it demonstrated in the documentation.

The HTML
------

First thing first, we need to add the necessary html for displaying the date picker.

Here is the corresponding stripped html :
{% highlight html %}
<div ng-controller="NewTodoCtrl as ntc">
    <div>
      <label id="label_end_time" for="end_time" class="control-label">End date</label>
        <div class="input-group">
          <input type="text" class="form-control" id="end_time" name="end_time"
            placeholder="End date" data-ng-model="ntc.todo.end_time" 
            data-datepicker-popup="" data-is-open="opened">
          <span class="input-group-btn">
            <button type="button" class="btn btn-default" ng-click="ntc.open()">
              <i class="glyphicon glyphicon-calendar"></i>
            </button>
          </span>
        </div>
    </div>
</div>
{% endhighlight %}

You should see something similar to this :

{% img center /assets/posts_images/html_picker.png %}

What is important in this snippet :

Since we are using AngularJS, we need a controller that will handle the date and
the popup of the date picker.
Mine is named *`NewTodoCtrl`*.

Inside the input element :
  
  + Adding a model which will contains the date populated by the date picker. The model is contained inside the controller. *`data-ng-model=ntc.todo.end_time`*
  + *`data-picker-popup`* which is used to handle the date picker as a popup.
  The default date format is used here but you can change it.
  + *`data-is-open="opened"`* The variable *`opened`* is used to notify the view from the controller for opening the popup.

The calendar button next to the input element is calling an *`open`* function contained in the controller.
The controller will then modify the opened variable to *`true`* for displaying the popup.

The controller
------

The controller is pretty simple, we just initialize the date used as the model in the constructor and corresponding to the *`data-ng-model`* in the view.
A function *`open`* is used to open the popup containing the date picker.
We use the *`$scope`* to modify the *`opened`* variable from the view and *`$timeout`*
to delay the execution with the default timeout of the navigator.

{% highlight coffeescript %}
class NewTodoCtrl

  constructor: (@$scope, @$log, @$timeout) ->
   @$log.debug "constructing NewTodoCtrl"
   @todo = {}
   @todo.end_time = new Date()

  open: () ->
   @$log.debug "Opening the date picker"
   @$timeout (=>
     @$scope.opened = true
   ) 
{% endhighlight %}

With all of this, you should now have an input element that contains the current
date selected from a popup date picker.

Current date by default in the input :

{% img center /assets/posts_images/html_picker2.png %}

When clicking on the calendar icon, the popup date picker is displayed :

{% img center /assets/posts_images/html_picker3.png %}

You can test the example as a JSFiddle : [here][here]


[AngularUI Bootstrap]: http://angular-ui.github.io/bootstrap/
[here]: http://jsfiddle.net/bXFdM/106/
