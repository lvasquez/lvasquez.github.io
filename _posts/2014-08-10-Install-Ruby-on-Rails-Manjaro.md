---
layout: post
title: Install Ruby on Rails on Manjaro Linux
comments: true
description: How to install Ruby on Rails on Manjaro Linux
published: true
---


Rails requires nodejs package so we need to install nodejs

{% highlight text %}
sudo pacman -S nodejs
{% endhighlight %}

 Now we need to install ruby

{% highlight text %}
sudo pacman -S ruby
{% endhighlight %}

Ruby includes RubyGems, RubyGems will install the gems to a directory inside your home directory, something like ~/.gem/ruby/2.1.0 The commands provided by the gems you installed will end up in ~/.gem/ruby/2.1.0/bin. For the programs installed there to be available for you, you need to add ~/.gem/ruby/2.1.0/bin to your PATH environment variable.

So wen can add that directory to our PATH, so fist we need to open ~/.bashrc file

{% highlight text %}
nano ~/.bashrc
{% endhighlight %}

and add the this code

{% highlight text %}
if which ruby >/dev/null && which gem >/dev/null; then
    PATH="$(ruby -rubygems -e 'puts Gem.user_dir')/bin:$PATH"
fi
{% endhighlight %}

save it, now we need to restart our shell for the changes to take effect. You can do this by opening a new terminal window or by running exec $SHELL in the window you already have open.

{% highlight text %}
exec $SHELL
{% endhighlight %}

now we can proceed with installing rails, but before that we need to run the following commands as root

{% highlight text %}
gem install rails --no-document
{% endhighlight %}

and the we can update the gems

{% highlight text %}
gem update
{% endhighlight %}

we can check all these tools are successfully installed

{% highlight text %}
ruby -v
gem -v
rails -v
rake --version
{% endhighlight %}

now wen can start to create our first application:

{% highlight text %}
rails new testapp
{% endhighlight %}

{% highlight text %}
cd testapp
{% endhighlight %}

{% highlight text %}
bin/rails server
{% endhighlight %}

![alt tag](http://lvasquez.github.io/images/rails-testapp.png)

References:

* <a target="_blank" href="http://blog.samsonis.me/2009/06/install-ruby-on-rails-on-archlinux/">http://blog.samsonis.me/2009/06/install-ruby-on-rails-on-archlinux</a>
* <a target="_blank" href="https://wiki.archlinux.org/index.php/ruby">https://wiki.archlinux.org/index.php/ruby</a>
* <a target="_blank" href="https://wiki.archlinux.org/index.php/Ruby_on_Rails">https://wiki.archlinux.org/index.php/Ruby_on_Rails</a>
* <a target="_blank" href="http://guides.rubygems.org/faqs/">http://guides.rubygems.org/faqs/</a>









