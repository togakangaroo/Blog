# Using new and this
 
This is the pattern that a lot things recommend
 
    function Person(firstName, lastName) {
      this.firstName = firstName;
      this.lastName = lastName;
      this.name = function() { return this.firstName + " " + this.lastName;}
    	this.printName = function() { console.log("my name is", this.name())}
    }
    var george = new Person("George", "Mauer");
 
What do you think that `new` does? It's not what you think. This is what `new` actually does
 
    var fn = new Function();
    fn.__proto__ = Person; //chrome only - each browser has a different syntax for this step
    var res = Person.call(fn, firstName, lastName);
    return (res === undefined) ? fn : res;
 
Huh? But that's insane.....exactly.
 
What about `this`? How can that be confusing.
 
The problem is that despite what you think `this` doesn't reference scope. It is really just a parameter like any other. This becomes a lot more clear if you realize that
 
    myFunc(1,2,3); 
 
is really just a facade for how javascript **really** invokes methods
 
    myFunc.call(whateverThisIs, 1, 2, 3);
    
So all that `this` is, is just the first parameter passed into `.call()`. That's it - no magic, no language-support, it's just a parameter.

Worse, if you use the simpler syntax, javascript will pick what the value of that parameter should be at runtime. The rules aren't terribly complex but they're often not what you want. As an example:
 
    george.printName(); //equivalent to george.prototype.printName.call(george)
    var printName = george.printName();
    printName();        //equivalent to george.prototype.printName.call(window);
    
And it gets wierder if you consider strict mode.
 
A lot of libraries **also** take guesses at what `this` should be.
 
    $('button').click(george.printName); //this won't work because "this" is the button element and doesn't have name()
 
`this` is therefore a parameter like any other and by using it you're trusting people to always pass in the correct value. And other people know less about `this` than you do!
 
so you **could** always use the `fn.call()` syntax. Or you could
 
# Not Use this or new
 
The same code but without these keywords:
 
    function createPerson(firstName, lastName) {
    	var _p = {
    		 firstName: firstName
    		,lastName:  lastName
    		,name:      function() { return _p.firstName + " " + _p.lastName}
    		,printName: function() { console.log("My name is", _p.name())}
    	};
    	return _p;  // I like an underscore prefix to indicate "this is what I'm buiding here"
    }
    
    var betterGeorge = createPerson("George", "Mauer");
    
And bam! No more wierd implicit parameters, callbacks and everything else works great forever!
 
# To Be Fair
 
Sometimes you can't get away from it. If you're in a jQuery callback for example you might have to use `this` because that's how the framework works. But please, **please** don't write your own code to use it.
 
Also, there ARE a few benefits to the `new`/`this` thing.
 
* In some cases referencing `this` is slightly faster than referencing a variable in a parent scope. This difference is very slight and basically matters only if you're writting the next underscore or jQuery. Your project doesn't need it. Seriously Mario, your bottleneck is in another castle.
* If you need to [support a browser without `Object.create`](http://caniuse.com/#search=object.create) there is no other way to wire up prototype inheritence cross-browser. This is another thing that unless you're building an extensible framework you probably don't need. If you need to share properties or methods every library has a version of the [extend helper](http://api.jquery.com/jQuery.extend/) that will work for this.
* Finally, there's the fact that when logged to a console, things with a prototype look **slightly** nicer and are labeled as their prototype rather than as "Object". This is valid but a tiny, tiny issue.
 
As a whole these benefits certainly don't excuse introducing two whole new concepts into your code, especially concepts that are as confusing and poorly understood as `new` and `this`.

I think it is probably only fair to amend that I'm usually opposed to javascript class systems in general. They require the use of `new` (though not necessarily of `this` - I'll write up how later)
and between function composition and the extend pattern are generally not necessary for either organizing or building performant code in 90% of cases.
