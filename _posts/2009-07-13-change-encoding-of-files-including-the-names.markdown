---
title: Change encoding of files (including the names)
layout: post
categories:
 - bash
 - encoding
 - iconv
 - iso-8859
 - utf
 - unicode
---

Recently, at work, I had a bunch (too many to handle manually) of
ISO-8859-1 encoded YAML-files, which I had to convert to UTF-8.

I knew that **iconv** took care of that, and with a for-loop I went
through all files and converted them.

{% highlight bash %}
for file in *.yml; do
  iconv -t UTF-8 -f ISO-8859-1 "$file" > "new/$file"
done
{% endhighlight %}

The file contents was converted correct. But the file names however
had question marks instead of some special characters (å, ä and ö). I
solved that with this little hack.

{% highlight bash %}
for file in *.yml; do
  iconv -t UTF-8 -f ISO-8859-1 "$file" > "new/$(echo $file | iconv -t UTF-8 -f ISO-8859-1)"
done
{% endhighlight %}

Note that if you find yourself using this sometimes and want to make a
script of it, you should probably refactor out the common call to
iconv and make the formats available to specify as arguments.
