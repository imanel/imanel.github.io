---
layout: post
title: Cucumber testing for multiple users - continuation
---

A little over a month ago you could read my article about real-time applications testing using Cucumber. In the meantime a new version of Cucumber emerged with [Capybara](http://github.com/jnicklas/capybara) support added. Since the previous method posed several problems during system to system migration (especially Snow Leopard hacks occasionally did not work in other systems) we'll try a new approach - this time working OOTB in every system.

As always, I will not describe the installation nor configuration process (you can find this in Capybara's documentation) so let's get straight to the practice. By default Capybara does not allow you to open multiple browser instances. According to the main developer - "it's impossible by design". As they say -  It's impossible. But doable ;) After taking a glance at Capybara's code (I must admit that it is written quite nice) I found a couple of points where you could bypass this annoying limitation.

It is time to get to work. Let's create a new set of steps in the features/step_definitions - call it e.g "selenium_steps.rb". For now let's assume that we want to open two browser instances (you can change that to different value if you want to). Create a method examining whether any browser instance exists (and creating new one if there's none):

{% highlight ruby %}
def check_selenium_browsers
  Capybara.instance_variable_set('@current_driver', :selenium)
  if $browsers.nil?
    $browsers = {}
    current_url
    $browsers[1] = get_selenium_browser
    set_selenium_browser(nil)
    current_url
    $browsers[2] = get_selenium_browser
    set_selenium_browser(1)
  end
end
{% endhighlight %}

The second line verifies that you use a correct test engine (in this case selenium). "current_url" method call guarantees that browser current state won't be lost during process. Next save to a variable state of the browser and then reset it so Capybara can open a new instance. Why not do that by ourselves? The reason is simple - Capybara uses quite a lot of tricks so you could run into potential problems after some code refactoring done by developers. For now this method is sufficient and guarantees stable (or at least predictable) behavior. Next call to "current_url" will open a new brower. Finally restore browser to its initial state. Look at the save/restore browser state functions code below:

{% highlight ruby %}
def get_selenium_browser
  {
    :session => Capybara.current_session,
    :driver  => Capybara::Driver::Selenium.instance_variable_get('@driver')
  }
end
{% endhighlight %}

As you can see, this method is very simple - it only acquires the current session (for Selenium) and "driver" (which is actually a reference to the Selenium server instance).

{% highlight ruby %}
def set_selenium_browser(browser_id)
  browser = $browsers[browser_id] || {}
  if browser[:session].nil?
    Capybara.instance_variable_set('@session_pool', {})
    Capybara::Driver::Selenium.instance_variable_set('@driver', nil)
  else
    Capybara.instance_variable_set('@session_pool', {"selenium#{Capybara.app.object_id}" => browser[:session]})
    Capybara::Driver::Selenium.instance_variable_set('@driver', browser[:driver])
  end
end
{% endhighlight %}

There's no magic here too - you just save the session state and override reference to the Selenium Server. Now we can write browser change functions with ease:

{% highlight ruby %}
Given /^I am using first browser$/ do
  check_selenium_browsers
  set_selenium_browser(1)
end

Given /^I am using second browser$/ do
  check_selenium_browsers
  set_selenium_browser(2)
end
{% endhighlight %}

This way we may use any number of browsers to test Juggernaut, WebSockets and different functionalities (even a simple, ajax communication between users, thus saving time needed to reauth in subsequent requests) in parallel.
Notice that during Cucumber shutdown phase not all browser instances will be closed (only the active one). To fix this add this code to the one of initialization (features/support) files:

{% highlight ruby %}
at_exit do
  $browsers && $browsers.each { |id, browser| browser[:driver].quit rescue nil }
end
{% endhighlight %}
