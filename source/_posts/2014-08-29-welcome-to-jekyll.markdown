---
layout: post
title:  "Using Selenium with Scala, play framework and specs2"
date:   2014-08-29 16:01:52
categories: scala play-framework specs2 selenium
---
At work, I used Selenium when I had to build a tool for writting and running automated tests.

I'm currently playing around with the Play framework in Scala and I thought it would be nice to be able to execute tests with Selenium.
I'm using [Specs2][specs2] as a testing framework and I wanted to use Selenium with it.

So let's do it!

***

Adding Dependencies
------

First, we need to add the dependencies for Selenium (the current version is 2.42.0) in the build.sbt file :

{% highlight scala %}
libraryDependencies ++= Seq(
  ...
  "org.seleniumhq.selenium" % "selenium-java" % "2.42.0" % "test"
)
{% endhighlight %}

To test everything is alright, just use *`sbt compile`* or *`activator compile`* (depending on what you are using).

***

Writing a simple test using Selenium
------

Next step : Creating a simple test for our application(Write a Scala file in the `test` folder). The goal of this test is actually quite simple! Just launching firefox on our local application to ensure that everything is working.

Here is the test :
{% highlight scala %}
import org.openqa.selenium.firefox.FirefoxDriver
import org.specs2.mutable._
import org.specs2.runner._
import org.junit.runner._

import play.api.test._
import play.api.test.Helpers._

@RunWith(classOf[JUnitRunner])
class IntegrationSpec extends Specification {

  "Application" should {

    "work from within a browser" in new WithBrowser(webDriver = WebDriverFactory(Helpers.FIREFOX)) {

      browser.goTo("http://localhost:" + port)
    }
  }
}
{% endhighlight %}

***

Running the test
------

Running the test is quite simple to, just use *`sbt test`* or *`activator test`*.

The output should be similar to this :
{% highlight scala %}

[info] Loading project definition from /home/.../Scala/project-name/project
[info] Set current project to Todo (in build file:/home/.../Scala/project-name/)
[info] 
[info] Application should
[info] + work from within a browser
[info] 
[info] Total for specification IntegrationSpec
[info] Finished in 7 seconds, 16 ms
[info] 1 example, 0 failure, 0 error
[info] Passed: Total 1, Failed 0, Errors 0, Passed 1
[success] Total time: 7 s, completed 29 août 2014 16:45:44
{% endhighlight %}

That's it! You should now be able to write tests that are executing in a browser.


[specs2]: https://github.com/etorreborre/specs2     
