# Start up to / stop down to

In many cases, calling just `(mount/start)` or `(mount/stop)` is enough.
But there may be circumstances, for instance in your tests, that you don't want to start all of your global states.

The `start` and `stop` functions can take one optional argument, a var.
The var points to a `defstate` that should be started (or stopped), including all its dependencies (or dependents).
It allows you to easily start or stop only a part of your application.

Note that the default behaviour is that the dependencies are calculated based on the _sequence_ of available `defstate`s as registered in mount-lite.
In effect, it might start (or stop) a bit more than the given `defstate` really depends on.
If you need more _graph-like_ behaviour, have a look at the [[mount.extensions.explicit-deps]] and [[namespace deps]] extensions.

> This "up-to" behaviour is unique to mount-lite.
> There used to be other options to influence what is started or stopped, but those turned out to anti-patterns and were rarely used.
> If you need more options though, have a look at the [extension point](05-extension-point.md) section.

An example:

```clj
(defstate a :start nil)
(defstate b :start nil)
(defstate c :start nil)

(start #'b)
;=> (#'user/a #'user/b)

(start)
;=> (#'user/c)

(stop #'b)
;=> (#'user/c #'user/b)
```
