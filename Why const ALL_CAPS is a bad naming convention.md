I like naming conventions fine. They can be important (eg in javascript PascalCase is the only way to know a function expects to be used with `new`) but I think a convention needs to have a good reason behind it. Otherwise it is just a "suggestion" and you're free to violate it if it suits your style better.

I do not believe there is a good reason behind the ALL_CAPS constants convention.

I could be wrong but I believe the origin of this convention goes back to C when constants were implemented using the preprocessor. This means that if you created a `#define max_length 5` and elsewhere had `request_max_length` it would be replaced to `request_5`. Therefore the convention evolved that ALL_CAPS would be used for preprocessor placeholders and nowhere else.

This reasoning stopped making sense the second we stopped having dumb preprocessors, but the convention persisted. When I grill people on why they usually say they like it because it lets them easily see "which variables shouldn't be assigned to". I don't see how this logic is valid.
These same people have no problem creating a field with `readonly string camelCase` even though you have the same situation!
The compiler will tell you when you can't assign to a variable. Visual Studio will tell you immediately
We don't do functional programming, but the ideas from FP are still very valuable. The majority of variables shouldn't be assigned to multiple times.
Additionally, writing something in ALL_CAPS naturally makes it be the first place people look, when in reality constants are usually one of the least important parts of your code for communicating your intent.
