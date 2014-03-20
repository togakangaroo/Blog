To start with, I like naming conventions. I am a big fan of using naming conventions [to communicate aspects of my code that are not otherwise easily expressed](https://github.com/togakangaroo/Blog/blob/master/javascript-on-this-and-new.md). 

That last bit is the important part, a naming convention should communicate something that *is of value* to know and be *difficult to communicate another manner*.

A perfect example is the javascript naming convention to camelCase all variable and function names *unless* the function is intended to be invoked using the `new` keyword when you PascalCase. This is obviously important information as simply invoking a constructor function will often throw an error, or even succeed while polluting the global namespace. It is also difficult with current javascript tooling to determine when a function you don't own is meant to be a constructor. So yes, lets use naming conventions here.

That being said, I do not believe there is a good reason behind the ALL_CAPS constants convention that I often see in Java, C#, and many other codebases.

As far as I know, the origin of this convention goes back to C when constants were implemented using the preprocessor. This means that if you created a `#define max_length 5` and elsewhere had `request_max_length` it would be replaced to `request_5`. A big problem! So the convention evolved that ALL_CAPS would be used for preprocessor placeholders and nowhere else.

This reasoning stopped making sense the second we stopped having dumb preprocessors, but the convention persisted. When I ask people why they use this convention I often hear something like it lets them easily see "which variables shouldn't be assigned to". I don't see how this logic is valid.

* These same people have no problem creating a field with `readonly string camelCase` even though you have the same situation!
* The compiler will tell you when you can't assign to a variable. Visual Studio will tell you immediately
* Even in non functional languages like C# and Java, the ideas from FP are still very valuable. The majority of variables should be treated as immutable. If anything, the naming convention should be reversed as it's usually more useful to understand that a variable is reassigned later than that one isn't.

To that last point, writing something in ALL_CAPS naturally makes it be the first place people look, when in reality constants are usually one of the least important parts of your code for communicating your intent.
