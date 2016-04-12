---
title: Workshop prerequisites - Brew for Mac
layout: default
---

# Installing Git and Brew on Mac 

{: .lead}
Some developer tools need to be installed on Mac.

# Xcode

This will provide git and gcc. See instructions
[here.](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and
then install the command line tools with:

{% highlight bash %}
xcode-select --install
sudo codebuild -license
{% endhighlight %}

# Brew

Details instructions for installing Brew can be found [here.](http://brew.sh/)
In brief:

{% highlight bash %}
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

# Other things

{% highlight bash %}
brew install gpg

# Mac version of C does not work for building Ruby
brew install gcc
{% endhighlight %}
