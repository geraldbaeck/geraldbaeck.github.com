---
layout: post
title:  "Install Sublime Text 3 Package Control"
date:   2013-11-19
categories: sublime3
---
{% include JB/setup %}

- Go to Preferences => Browse Packages… menu
- Go up a folder into the Installed Packages/ directory
- Download [Control.sublime-package](https://sublime.wbond.net/Package%20Control.sublime-package) and copy it into the Installed Packages/ directory
- Restart Sublime Text

Even easier:
- Open console via ctrl+´ or the View > Show Console menu
- Paste this nasty litte piece of python code into the console
{% highlight python %}
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
{% endhighlight %}

via [sublime.wbond.net](https://sublime.wbond.net/installation#st3)