SafeDweet - an introduction
===========================

So you wanted to build yourself your own sous vide cooker.  
And you are quite the maker.  
You bought an Arduino and parts, put it together, and then decided to go all-out:  
Being the awesome coder you are, you connected your cooker to the cloud, and wrote a small app to control the temperature from your phone.

Time for a test.  
You shrink wrap a couple fillets, put it in your cooker, and go out for a few hours.  
Coming home, you start to smell the grill from two blocks away.  
Wait, grill? This is a water cooker.   
That grill smell - that's the smell of your house toasted.

WHAT HAPPENED?

You decided to control the cooker using the beautifully simple web API of dweet.io.  
But you were too cheap to buy a lock.  
And some bored hacker, scanning the latest dweets, decided it would be pretty fun to raise your cooker from 50 degrees centigrade to 250.

Fortunately, you didn't build that sous vide cooker yet.  
But if you do, SafeDweet was written exactly with you and your hacker friend in mind.


What is SafeDweet?  
SafeDweet is a small library that lets you send and receive messages to dweet.io, but all messages are authenticated to prevent tampering by unauthorized third parties.  
This means that if you set the temperature of your cooker, nobody will be able to change it.

How does it work?  
SafeDweet uses two tricks that are quite common in the world of electronic communication:  
It authenticates your message, to make sure that you sent it, and it prevents your message from being reused for malicious purposes.  
SafeDweet creates a cryptographically strong hash of your message, including your device name, an incrementing counter (we'll get to that later) and a secret key.  
The key must be known only to your device and your app.  
This hash cannot be reverse-engineered to find out the secret key (but there are still ways to hack that key).

What's that counter I mentioned?  
The counter is used to prevent attacks known as replay attacks.  
Let's say you raise your sous vide oven to 90 degrees for a few minutes, just to sterilize it.  
And then you set it down to 50 for cooking.  
If there were no replay protection, some hacker could resend your message that set the oven to 90.  
The message would be authentic - it was originally sent by you and has your digital signature.  
By using an incrementing counter, no message can be sent twice.  

What does it NOT do?  
SafeDweet does not encrypt your message.  
Anything you send can be read by a third party, unless encrypted in another method.  
As long as you are not sending anything confidential or offensive in your message, you are ALMOST ok.   

Why almost ok?   
I  mentioned that the hash can't be used for decrypting your password.  
But someone with a strong enough computer and enough desire and time can take your message and try to re-encrypt it, each time using a different password, until he finds one that creates the same hash you sent.  
This is what is called a brute force attack, and can take a long amount of time if the hash
is strong enough.  
Unfortunately, with the power of computers today, it works when someone really wants to put the effort into it,
and when people select really crappy passwords.

How do you work around that?  
Make sure you always send your messages to dweet.io through https, not http.  
This will encrypt the messages with SSL during transfer.  
And buy that lock from dweet.io.   
Otherwise your message can be read off their website.  
In the future, a useful extension to SafeDweet would be to add a mechanism to exchange passwords over a short time.  
If your password has a lifetime of a few minutes, you can be pretty confident that it won't be able to be exploited in time.

Should you use SafeDweet?  
Now is the time for the big WARNINGS and CAVEATS:  
1. No security is 100% safe, and the worst kind of security is a false sense of security.  
It's all too easy to make your life complicated for yourself with extra levels of protection, while leaving a nice big hole for the bad guys to walk through.  
2. I have just enough experience in security to know that the world is full of hackers who are smarter than me, have more free time than me, and are much more bored than me.  
Therefore, expect this tool to be hackable.   
3. This library is not yet beta quality, it is not yet alpha quality.  
It is basically a prototype, a proof of concept.  
This is a midnight project, and would not pass the scrutiny of my own code review, if done in the morning.  
It is way too inefficient in its memory use for most embedded applications.  
There are many critical holes in the code that need to be plugged - holes that can be exploited by a serious hacker.  
4. Currently only the Arduino half of the library is written.  
It is still missing that cool javascript library that will allow web sites and phone apps to talk to IoT devices running SafeDweet.

What next?  
Please download and play with this library.  
Help plug those gaping holes.  
Help make it a real, viable tool that will allow more makers to make connected devices that won't get hacked
in a day.  
If you have suggestions to improve the API, please do so.  
And write that javascript library!   
Personally, I am clueless when it comes to web languages, so it will take me ages to port it myself.

One last thing:   
About that shared password that is needed between the IoT device and your app.   
How do you do that?  
That's a totally different topic, with many different solutions.  
My personal favorite is the Google Chromecast (trademark?) style solution:  
Have your device come up as a wireless access point.  
Write a small app that connects to your device securely (or in the case of iOS, instructs the user how to connect).
Make sure that it is really you connected to the device - flash some LEDs, print some characters on the display, whatever.  
Exchange a password and give the device the credentials it needs to connect to your regular AP.  
Reboot the device as a wireless station, connecting to your AP and through it to the internet.  
Job done.
Another option suitable for IoTs:   
Create a random password and glue a sticker at the bottom of the device with a barcode.  
Other than that there are lots of more complex solutions for key exchange, but for makers I like the simpler approaches.


One final note, in case you were curious:  
I never did get around to building that sous vide Arduino cooker I keep dreaming about.  
It remains a mental exercise, spawning ideas like this SafeDweet library.  
As much as I mildly fear hackers, I have a much greater fear of mixing high voltage, water, and a house full of small kids.

Enjoy,
maklotski
Sept. 2015
