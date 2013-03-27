---
layout: post 
title: Debugging on AppFog
---

A big plus when deploying to a platform as a service (<a href="http://en.wikipedia.org/wiki/Platform_as_a_service">PaaS</a>) is that you're pushing the responsibility for a lot of the systems administration tasks to the PaaS provider, which ideally leaves you to concentrate on writing your application.  As a result however, on services like <a href="http://www.appfog.com">AppFog</a> or <a href="http://www.heroku.com">Heroku</a>, you no longer have direct access to the environment your application is running on.  So you can't SSH into a box and grep through logs, which some people are used to in their debugging workflow.

But AppFog and these other PaaS providers of course provide you with resources to help you debug applications without SSH access to a server.  And probably the two big places where you'll end up needing to debug problems on AppFog are application deployment and runtime errors.

Before diving into these two categories, it's probably best to mention the one thing to do that will be helpful in either case; make sure your code works in a local development environment.  If your code doesn't work at all, deploying to the cloud isn't going to help (it's not a magic cloud).  Though it's not definitive, you'll have a better idea if your code should run in another environment.

As an aside, from what I can tell, AppFog is a pretty awesome service.  I also think that a lot of the people working there seem like nice people too, but I can't really be sure.  And Portland is pretty nice as well even if you're not down with precipation - lots of great <a href="http://www.foodcartsportland.com/">food</a>, and they even have a <a href="http://www.barflymag.com/bar/blitz-southeast.html">load balancing bar</a> where you can play ping pong (lame barely inside joke).


### Application Deployment

After you've got your application running in a development environment, you'll want to start by checking out the [documentation](https://docs.appfog.com/languages/ruby#gems) for specific things to watch out for overall, as well as with specific languages or frameworks.  For example:

* For <a href="https://docs.appfog.com/services">database services</a>, you extract database credentials through the VCAP_SERVICES environment variable.
* For Ruby applications, there are some Gemfile features that are <a href="https://docs.appfog.com/languages/ruby#gems">not yet supported</a>.
* Some languages or frameworks are detected based on convention - e.g. Node applications are detected by the presence of an app.js file, and the main application file in Sinatra applications is detected by a text search for the string "require 'sinatra'" in a Ruby file.

So definitely read the documentation as much as possible, because that will make your life easier when you deploy your application.   

Then after you actually deploy your application with `af push` and select all the application options you want, you'll see a bunch of logging information output to the terminal.  The deployment logging has improved over time and will probably continue to improve, so if your application doesn't deploy, pay attention to the log messages.  You may be told exactly what the issue is, or you might get a hint about what the problem may be.

### Runtime Errors

If your application deploys, then awesome, you have a running application on AppFog.  But if you get a runtime error or your application crashes (in which case you may get a 404 error as your application is automatically restarted), a good place to look are your log files.  If you run `af logs <appname>`, the logs for you application will scroll by, and you can grep through the output, or redirect it to a file to examine later or pass on to other people.  The logs are a snapshot, not streaming, but I would guess that AppFog will offer a logging add-on at some point in the future as well.

You may see something like a failed database connection which would point to the fact that you're not using VCAP_SERVICES correctly.  Or there could be an out of memory error message for your Java application.  Or your Node application is having trouble because it's not using the port defined in VCAP_APP_PORT to listen on.  Like deployment errors, definitely check to see if there's specific errors that are being captured in the logs by the application and the platform, and hopefully that will help you resolve any runtime errors.

If there's no error in the logs, you can always add <a href="http://stackoverflow.com/a/189570">good old print statements</a> to your code to narrow down where the problem is and check the logs to see how far your application gets before running into trouble.  Isolating what's causing the error can also help you if you post to someplace like <a href="http://stackoverflow.com/questions/tagged/appfog">StackOverflow</a> or the <a href="http://stackoverflow.com/questions/tagged/appfog">AppFog Users Group</a> because other people may recognize from the few lines of code you post what the problem is.

 
### Miscellaneous

Other random things to watch out for on AppFog include:

* **DB Service Limits** - if you hit your database size limit, your priveleges will drop to read and delete on your database until you clear up some space.

* **Rails applications** - problematic gems (in the AppFog environment) in your Gemfile can be a reason why an application won't deploy.

* **Java applications** - deployment issues, or crashes could be a memory issue.  Check your logs and if you see an out of memory error, increase the allocated memory and try again.

* **Runtimes** - applications are deployed to a particular runtime (e.g. Ruby 1.9.3), so try and develop using the same runtime that you're going to deploy to.   You can get a list of available runtimes by running `af runtimes`.

This is not an exhaustive list by any means, so again, check out the documentation and also search through the user group to see if you're running into a known issue, if there's a workaround, or you're just doing it wrong.


### The End.

PaaS is really awesome, and more and more developers continue to use these servicess.  But what can be frustrating is figuring out whether you're having problems because of your application or because of the platform your application is running on.  There's a trade off when you offload a lot of the application envrionment setup and management to the AppFogs and Herokus of the world.  You get to concentrate more on writing code, but on some services, you also don't have that ability to dig in and SSH into a box and grep through the syslog or Apache error logs, and you can't install/upgrade a particular library.

But each platform should have tools, documentation, and resources to help you work through issues you may run into, so you should familiarize yourself with these resources.  Hopefully you'll be able to figure out what the problem is on your own (which always <a href="http://en.wikipedia.org/wiki/Feels_Good">feels good</a>), or you'll at least have more information to provide others when you ask for help.


### RTFM and Other Resources

[AppFog Documentation](https://docs.appfog.com/).  
Official AppFog documentation - especially read the language specific documentation for whatever type of application you're working on and the documentation on using the af command line tool.

[AppFog Users Group](https://docs.appfog.com/).  
Community forum where you can post questions if you get stuck.
