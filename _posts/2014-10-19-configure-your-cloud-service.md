---
layout: post
title: Configure Your Cloud Service
---

API keys, passwords, etc. How do we store and pass these data to our cloud service?

Store them with code and manage them by a revision control system?
Not a good idea.

How about environment variables?
The twelve-factor app
[recommends](http://12factor.net/config)
this.
MySQL End-User Guidelines for Password Security
[suggests against](http://dev.mysql.com/doc/refman/5.7/en/password-security-user.html)
this recommendation.
My view was that since on reasonably recent Unix systems,
environment variables is
[only readable](http://security.stackexchange.com/questions/14000/environment-variable-accessibility-in-linux/14009#14009)
to the user (`euid`) running the process,
it is fine to use environment variables
until
[Shellshock](http://en.wikipedia.org/wiki/Shellshock_(software_bug))
has made me uneasy about using environment variables.
I am not worrying about an attacker stealing contents of environment variables,
but worrying that we might give an attacker an opportunity to exploit Shellshock.
To prevent this, we will need to...

* Sanitize environment variables,
* Remove unrecognized environment variables, and
* Make sure that `bash` is patched.

However, it is hard to implement these countermeasures right
particularly because environments are "implicit" by default.

The plain old files are arguably the safest and not least convenient method so far.
We will deploy `config.txt` the same way we deploy `my-server`,
and we may now remove all environment variables before we start a new process.

{% highlight bash %}
# Start my-server...
env - my-server --config /path/to/config.txt
{% endhighlight %}

{% highlight python %}
# Launch a sub-process...
subprocess.Popen(
    ['my-server', '--config', args.config], env={})
{% endhighlight %}

Oh, by the way,
coreos [etcd](https://coreos.com/using-coreos/etcd/) looks promising.
