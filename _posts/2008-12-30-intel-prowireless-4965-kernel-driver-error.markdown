---
title: Intel PRO/Wireless 4965 kernel driver error
layout: post
categories:
 - driver
 - intel
 - linux
 - wireless
 - wlan
---

In recent kernel versions there seems to be some problem with the
**Intel PRO/Wireless 4965** driver.

**dmesg** shows this message:
<pre>iwlagn: iwlwifi-4965-2.ucode firmware file req failed: Reason -2
firmware: requesting iwlwifi-4965-2.ucode</pre>

Looking in **/lib/firmware** there is a file named
**iwlwifi-4965-2.ucode**. So there's obviously something wrong with this
file.

Fetching the ucode file from
[Intel](http://intellinuxwireless.org/?n=Downloads) and placing it in
**/lib/firmware** solved the problem for me.

This is how you do it:
{% highlight bash %}
# Download zip file from http://intellinuxwireless.org/?n=Downloads

# Unpack it
tar -xvzf iwlwifi-4965-ucode-228.57.2.23.tgz

# Move it
mv iwlwifi-4965-ucode-228.57.2.23/iwlwifi-4965-2.ucode /lib/firmware/

# Restart the daemon
/etc/init.d/net.wlan0 restart
{% endhighlight %}

You should now be able to use your wireless again.
