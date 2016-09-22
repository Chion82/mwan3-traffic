mwan3-traffic
-------------
*mwan3-traffic* is a patch for mwan3 on OpenWRT, with traffic load monitor added. You can set maximum bandwidth for each interface, and the interfaces with overloaded traffic will be automatically turned down.

Say, assume you have an internet cable (100 Mbit/s) from ISP A and another (also 100 Mbit/s) from ISP B, and both are already connected to your OpenWRT router with mwan3 load balance configured. However, network B usually sucks and you prefer using network A prior to B. Setting a higher `Weight` for member `A` in your mwan3 configuration might solve your problem but it turns out that you can't make full use of the combined bandwidth from both cables when using multithreading download tools. With *mwan3-traffic* patched for your existing mwan3, you can set the `bandwidth` of your interface `A` to 100 Mbit/s, followed by setting a new metric for member `B` which is higer than the metric for member `A`. By doing this, generally, all outgoing data will be sent via A, and only when the traffic of A is overloaded, any later outgoing requests will be switched to B.

Usage
-----
* Setup mwan3 config for your OpenWRT router as normal.

* Use different metrics for your `members` in mwan3 if you want to set a higher priority for one of your networks.

* Copy the `etc` and `usr` directories to the `/` path on your router.

* Manually modify `/etc/config/mwan3`: Add `option bandwidth '13107200'` to the `config interface XXX`.

Here is an example:

```
config interface 'WAN1'
	option enabled '1'
	option reliability '1'
	option count '1'
	option timeout '2'
	option interval '5'
	option down '3'
	option up '3'
	option bandwidth '13107200'
	list track_ip '180.97.33.108'
	list track_ip '119.84.77.123'
```

Change `13107200` to the real bandwidth of your interface, in bytes per second.
