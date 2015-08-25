---
layout: post
title: "JavaScript Getters and Setters"
date: "2013-07-25"
---

Last year I made the mistake of blindly accepting to speak at a local
JavaScript meetup on a topic I knew nothing about: Getters and Setters in 
JavaScript. I had been reading the O’Reilly tome on JavaScript and, on reading 
the section on getters and setters, thought to myself “Hey, that sounds kind of 
cool [turn page and continue to next section]”. So I accepted, and started 
researching the details.

Now my mistake wasn’t that I agreed to speak on a topic I didn’t know about. In
fact, I think its a rather good way to add some external pressure to the
learning process(within reason, of course). No, the mistake was that I agreed
to speak on a topic that soon I discovered to be totally worthless. The more
research I did, the less value I perceived this feature to have. Who in their
right mind would use something that useless? No one[^1]. Since nobody uses them,
I couldn’t find any examples or use cases, other than some really trivial ones.
So now I’m booked to tell people about a new feature they didn’t know about
that they shouldn’t use. Shit.

I didn’t want to just get up there and waste people’s time, so I started trying
to find interesting and valid use cases for them. Thats when I ran into this:
[http://www.joshondesign.com/2012/02/13/javascript-getters-and-setters-why-u-no-use-dem](http://www.joshondesign.com/2012/02/13/javascript-getters-and-setters-why-u-no-use-dem).
Someone else is searching for these in the wild, too! But they didn’t find
anything either. He had some interesting ideas, though, so I got a second wind.
It turns out that this feature is actually useful, but there’s no widespread
adoption for two major reasons:

* It’s not cross-browser and **can’t be shimmed** (that's the big one)
* It has little perceived value and is misleading if you have prior understanding of getters and setters.

This is what I learned throughout the course of researching. I’m getting the
boring stuff out of the way in the beginning, so if you’re not interested in
compatibility or syntax, [skip here](#why-use-getters-and-setters).

### Compatibility
Defined in the spec, and implemented in almost every major
browser(not IE8), I found that very few are actually using the feature. At
first glance one would just do what you might with a slew of other
unimplemented, but specified, features: write a shim and get on with the
future. The problem is that shims are really helpful when it comes to new
methods that have yet to be implemented, but the syntax for defining getters
and setters requires a change in how the code is interpreted, so it can’t be
shimmed.

### Syntax

Getters and setters are referred to as accessor properties, as opposed to data
properties that we’re familiar with. The syntax for declaring these in object
literals goes like this:

{% highlight javascript %}
var newObj = {
 count: 3,
 get myCount(){
  return this.count;
 },
 set myCount(newVal){
  this.count = newVal;
 }
};
{% endhighlight %}

The getters and setters are functions, but they’ll behave just like a regular property. Here’s an example(the commented lines are the console output):

{% highlight javascript %}
newObj.count
// 3
 
newObj.myCount
// 3
 
newObj.myCount = 5
newObj.myCount;
// 5
 
newObj.count
//5
{% endhighlight %}

In terms of syntax and basic behavior, that’s all there is to it.

### Why use getters and setters?

Since most JavaScript developers don’t use getters and setters (not a part of
common practice, at least), I had to answer this question. I looked to a
language that uses getters and setters extensively: Java. What are the benefits
in Java? Data Hiding in the form of limiting access to private members (think
validating a new value before setting it). Encapsulation in the form of
providing an interface, which is a crucial OO paradigm dating back to the GoF
book.

#### Data Hiding
Well it turns out that getters and setters in JavaScript don’t really have these benefits at all.

{% highlight javascript %}
var obj = {
   _hiddenProp: undefined,
   get prop() { return this._hiddenProp; },
   set prop(newVal) {
       if(newVal > 0) {
           this._hiddenProp = newVal;
       }
       else {
           throw new Error("Must Be Positive!");
        }
   }
};
 
obj.prop = 12
obj.prop       // returns 12
obj.prop = -2  // Error gets thrown, YAY!!
 
obj._hiddenProp = -2
obj.prop       // returns -2, commence app breakage
{% endhighlight %}

If you want to do anything like data hiding in Java, you can do it, but it won’t be using the getters and setters.

#### Encapsulation
You are providing an interface for developers to adhere to, kind of. Can you
change the implementation of the getters and setters without having the
developer having to care? Yes! Does it provide you anything that you couldn’t
have done using a data property that references the same function? Noooope. The
only thing that changes is the syntax, but it’s definitely not syntactic sugar.
Consider a developer is debugging some code and gets to the following line:

{% highlight javascript %}
someObj.aProp = 63;
{% endhighlight %}

That looks pretty straight-forward, but upon further inspection:

{% highlight javascript %}
someObj.aProp = 63;
{% endhighlight %}

"What the hell?" the developer is sure to think, because we expect a certain
behavior from properties. We don’t expect them to do work, because that’s what
functions are for. Why not just have:

{% highlight javascript %}
someObj.aProp(63);
someObj.aProp();
// “C”
{% endhighlight %}

It’s much more clear what’s going on. Even if the developer doesn't know whats
going on, they know that they can find that method and check it out.

The major benefits of getters and setters in Java don’t carry over to
JavaScript at all, so what are these things good for anyways?

### Taking advantage of side-effects

We can use getters and setters to keep track of property access behind the scenes.

{% highlight javascript %}
var obj = {
   _readCount: 0,
   _writeCount: 0,
   _hiddenProp: undefined,
   get prop() {
       return this._readCount++, this._hiddenProp;
   },
   set prop(newVal) {
       this._writeCount++;
       this._hiddenProp = newVal;
   }
};
{% endhighlight %}

Above, I’m making the getters and setters behave like data properties to the
developer, but the object can actually keep track of how many times the
property has been read or written to. This has some interesting implications
from a logging or usage tracking standpoint. Alternatively, we could adjust the
code to reset a property value after a certain amount of reads, or a certain
amount of time. (I'd love to hear ideas about use cases for property values
that time out.)

{% highlight javascript %}
var obj = {
   _readCount: 0,
   _writeCount: 0,
   _hiddenProp: null,
   get prop() {
  if(this._readCount < 5) {
           return this._readCount++, this._hiddenProp;
       }
       else {
            this._readCount = 0;
            this._hiddenProp = null;
            return this._hiddenProp;
       }
   },
   set prop(newVal) {
       this._writeCount++;
       this._readCount = 0;
       this._hiddenProp = newVal;
   }
};
 
 
//or
 
var obj = {
   _expireTime: null,
   _hiddenProp: null,
   _propLifetime: 60 * 1000, // 60 seconds
   get prop() {
       if(this._expireTime && new Date().valueOf() < this._expireTime){
           return this._hiddenProp;
   }
  else {
      this._expireTime = null;
           this._hiddenProp = null;
           return this._hiddenProp;
       }
   },
   set prop(newVal) {
       this._expireTime = Date.now() + this._propLifetime;
       this._hiddenProp = newVal;
   }
};
{% endhighlight %}

### Stringify-ability
This is where we start to get into some new possibilities. When you stringify
an object, functions are never included. But, if you stringify an object that
has getters, the getter function is invoked and the return result is
stringified(assuming you're not returning a function). Now you have the
opportunity to write a logging object that has all sorts of computed properties
(based on time and the user's environment, the current state of the page,
etc.), and all you need to do to calculate that object and serialize it for a
POST request is pass it to JSON.stringify().

Imagine wanting to log some page data on a particular event (an error maybe).
For simplicity, lets only log the time of the event, the current hash value of
the url.

{% highlight javascript %}
var logObj = {
 get time(){ return new Date(); },
 get hash(){
  var href = document.location.href;
  var idx = href.indexOf('#');
  return idx > -1 ? href.substr(idx+1) : '';
 }
};
{% endhighlight %}
### Where else?

I spent a lot of time discussing the concept and potential application of
getters/setters with coworkers and would love to hear about your ideas. Little
working prototypes would be cool, too...


_footnotes_

[^1]:  This isn't entirely true. The node community does utilize accessor properties thanks to the stability of only having to account for V8. Check out [should.js](https://github.com/visionmedia/should.js/) for a production example of how this could be used.
