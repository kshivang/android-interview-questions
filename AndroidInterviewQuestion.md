# Android Important Android Question Compilation

## Basic Question

**Q. How to launch an activity in android?**

**Ans:** Using with intent, we can launch an activity.

```java
Intent intent = new Intent(this, MyTestActivity.class);

        startActivity(intent);
```

**Q. What are Intent filters?**

**Ans:** Intent filters are filterout the intents.


Specifices the types of intents that an activity, service, or broadcast receiver can respond to. 
An __**intent filter**__ declares the capabilites of its parent components - what an activity or 
service can do and what types of broadcasts a receiver can handle. It opens the component to 
receiving intents of the advertised type, while filtering out those that are not meaningful for 
the component.

**Q. What is singleton class in android?**

**Ans:** A class which can create only an object, that object can be share able to all other
class

This is very important class for memory and task managemet.

__For example, Volley RequestQueue should be implemented using Singleton, so that we would have
single queue for entire app cycle__

**Q. What is nine-patch images tool in android?**

**Ans:** We can change bitmap images in nine sections as four corners,four edges and an axis

This is a important tool for dynamic images.

__For example, Message bubble of whatsapp__


**Q. What is a Sticky Intent in android?**

**Ans:** Sticky Intent is also a type of intent which allows the communication between a function
and a service. 

__For example,sendStickyBroadcast() is perform the operations after completion of 
intent also.__


**Q. Define the appliction resource file in android?**

**Ans:** JSON,XML bitmap.etc are application resources.You can injected these files to build process 
and can load them from the code.


**Q. How to update UI from a service in android?

**Ans:** Use a dynamic broadcast receiver in the activity, and send a broadcast from the service. 
Once the dynamic receiver is triggered update UI from that receiver.


**Q. What are the type of flags to run an application in android?**

**Ans:** 
```java
FLAG_ACTIVITY_NEW_TASK
FLAG_ACTIVITY_CLEAR_TOP
FLAG_ACTIVITY_SINGLE_TOP
```

`FLAG_ACTIVITY_NEW_TASK`: Start the activity in a new task. If a task is already running for the 
activity you are now starting, that task is brought to the foreground with its last state restored 
and the activity receives the new intent in onNewIntent().

This produces the same behavior as the "singleTask" launchMode value, discussed in the previous section.

`FLAG_ACTIVITY_CLEAR_TOP`: If the activity being started is already running in the current task, then 
instead of launching a new instance of that activity, all of the other activities on top of it are destroyed 
and this intent is delivered to the resumed instance of the activity (now on top), through onNewIntent()).

`FLAG_ACTIVITY_SINGLE_TOP`: If the activity being started is the current activity (at the top of the 
back stack), then the existing instance receives a call to onNewIntent(), instead of creating a new 
instance of the activity.

This produces the same behavior as the "singleTop" launchMode value, discussed in the previous section.

## Tricky Question

**Q. Under what condition could the code sample below crash your appliction?**
**How would you modify the code to avoid this potential problem? Explain your answer.**

```java
Intent sendIntent = new Intent();
    sendIntent.setAction(Intent.ACTION_SEND);
    sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
    sendIntent.setType(HTTP.PLAIN_TEXT_TYPE); // "text/plain" MIME type
    startActivity(sendIntent);
```

**Ans:** An implicit intent specifies an action that can invoke any app on the device able to perform 
the action. Using an implicit intent is useful when your app cannot perform the action, but other apps 
probably can. If there is more than one application registered that can handle this request, the user 
will be prompted to select which one to use.

However, it is possible that there are no applications that can handle your intent. In this case, your 
application will crash when you invoke startActivity(). To avoid this, before calling startActivity() 
you should first verify that there is at least one application registered in the system that can handle 
the intent. To do this use resolveActivity() on your intent object:

```java
// Verify that there are applications registered to handle this intent
// (resolveActivity returns null if none are registered)
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(sendIntent);
} 
```


**Q. The last callback in the lifecycle of an activity is onDestroy(). The system calls this method on 
your activity as the final signal that your activity instance is being completely removed from the system memory 
Usually, the system will call onPause() and onStop() before calling onDestroy(). Describe a scenario, though, 
where onPause() and onStop() would not be invoked.**

**Ans:** onPause() and onStop() will not be invoked if finish() is called from within the onCreate() method. 
This might occur, for example, if you detect an error during onCreate() and call finish() as a result. In such a 
case, though, any cleanup you expected to be done in onPause() and onStop() will not be executed

Although onDestroy() is the last callback in the lifecycle of an activity, it is worth mentioning that this 
callback may not always be called and should not be relied upon to destroy resources. It is better have the 
resources created in onStart() and onResume(), and have them destroyed in onStop() and onPause, respectively.

**Q. Suppose that you are starting a service in an Activity as follows:**

```java
Intent service = new Intent(context, MyService.class);             
startService(service);
```
where MyService accesses a remote server via an Internet connection.

**If the Activity is showing an animation that indicates some kind of progress, what issue might you encounter and
how could you address it?**

**Ans:** Responses from a remote service via the Internet can often take some time, either due to networking latencies
 , or load on the remote server, or the amount of time it takes for the remote service to process and respond to the 
request.

As a result, if such a delay occurs, the animation in the activity (and even worse, the entire UI thread) could be 
blocked and could appear to the user to be “frozen” while the client waits for a response from the service. This is 
because the service is started on the main application thread (or UI thread) in the Activity. 

The problem can (and should) be avoided by relegating any such remote requests to a background thread or, when feasible, 
using an an asynchronous response mechanism.

**NOTE WELL:** Accessing the network from the UI thread throws a runtime exception in newer Android versions which 
causes the app to crash.

**Q. Normally, in the process of carrying out a screen reorientation, the Android platform tears down the foreground 
activity and recreates it, restoring each of the view values in the activity's layout.**

**In an app you're working on, you notice that a view's value is not being restored after screen reorientation. What
could be a likely cause of the problem that you should verify, at a minimum, about that particular view?**

**Ans:** You should verify that it has a valid `id`. In order for the Android system to restore the state of the views
in your activity, each view must have a unique ID, supplied by the `android:id` attribute.


