 $kernel = new AppKernel('prod', false);
$request = Request::createFromGlobals();
$response = $kernel->handle($request);
$response->send();

 AppKernel.php 

 It allows you to register your bundles, and to change some major settings, like
the location of the cache directory or the configuration file that should be loaded. Its constructor
arguments are the name of the environment and whether or not the kernel should run in debug
mode

However, when looking at the Kernel and how it deals with all bundles,
including yours, it becomes apparent that bundles are mainly treated as ways to extend the service
container, not as libraries of code. This is why you find a DependencyInjection folder inside many
bundles, accompanied by a {nameOfTheBundle}Extension class

In the example above you saw that matthias_blog is the configuration key for settings
related to the MatthiasBlogBundle. It may now not be such a big surprise that this is true
for all keys you may know from config.yml and the likes: values under framework are
related to the FrameworkBundle and values under security (even though they are defined
in a separate file called security.yml) are related to the SecurityBundle. Simple as that!

As you can see, most of the work is done in the private handleRaw() method, and the try/catch
block is here to capture any exceptions

running a Symfony application means booting the kernel and
handling a request or executing a command, where booting the kernel means: loading all bundles
and registering their service container extensions (which can be found in the DependencyInjection
folder of a bundle)

After creating many bundles, I concluded that much of my work as a developer which is specific
for a Symfony application consists of writing code for exactly these things: bundle, extension and
configuration classes and compiler passes. When you know how to write good code, you still need
to know how to create good bundles, and this basically means that you need to know how to create
good service definitions. There are many ways to do this, and in this chapter I will describe most
of them. Knowing your options enables you to make better choices when looking for a dependency
injection pattern.