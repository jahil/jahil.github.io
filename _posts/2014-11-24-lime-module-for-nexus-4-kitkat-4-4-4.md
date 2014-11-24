---
layout: post
title: "LiME Module for Nexus 4 - Kitkat 4.4.4"
modified:
categories: 
excerpt:
tags: [volatility, kernel, forensics, module, android, mako]
image:
  feature:
date: 2014-11-24T17:59:25+04:00
---

[LiME ~ Linux Memory Extractor](https://github.com/504ensicsLabs/LiME) allows the acquisition of volatile memory from Linux and Linux-based devices.
Nexus 4 stock kernel doesnt have support LKM, you can build module against stock kernel unless you rebuild kernel with LKM support.

* Device:Mako
* Kernel Version: franco
* Android: 4.4.4 
* Build: KTU84P 

<a markdown="0" href="http://192.241.177.15/Nexus4/4.4/boot-r213.img">franco kernel</a>

## Usage
LiME utilizes the insmod command to load the module, passing required arguments for its execution.

{% highlight text %}
insmod ./lime.ko "path=<outfile | tcp:<port>> format=<raw|padded|lime> [dio=<0|1>]"

path (required):   outfile ~ name of file to write to on local system (SD Card)
        tcp:port ~ network port to communicate over

format (required): raw ~ concatenates all System RAM ranges
        padded ~ pads all non-System RAM ranges with 0s
        lime ~ each range prepended with fixed-size header containing address space info

dio (optional):    1 ~ attempt to enable Direct IO
        0 ~ default, do not attempt Direct IO

{% endhighlight %}

## Examples

Acquisition over the network
{% highlight text %}
$ adb push lime.ko /sdcard/lime-mako.ko
$ adb forward tcp:4444 tcp:4444
$ adb shell
# insmod /sdcard/lime.ko "path=tcp:4444 format=lime"
{% endhighlight %}

Nw on the host machine, we can establish the connection and acquire memory using netcat
{% highlight text %}
$ nc localhost 4444 > mako.lime
{% endhighlight %}

Acquisition to sdcard
{% highlight text %}
# insmod /sdcard/lime.ko "path=/sdcard/ram.lime format=lime"
{% endhighlight %}
