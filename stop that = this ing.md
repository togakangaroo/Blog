From our chatlogs at Surge (editted for berevity and to make me seem like a better debater/speller).

> GM: Can I just say, I hate 

>     that = this

>     or

>     self = this

> GM: Don't anyone do that

> GM: The pattern is fine, but at least give it a meaningful name

> JA: Hmmm.....I'll bite, devil's advocate

> JA: Why not folow an established convention? When javascript programmers see a variable name "that", they immediately know what's happening

> TS: anyone who has worked in JS for more than 10 seconds does :)

> GM: @TS -  right, they know that here is a variable passed in from a broader scope. However, when you use a variable you care far more about what it reperesents than where it came from, and in order to learn what "that" represents you have to scan the whole file, find the line that assings `var that = this;` then figure out in your head what `this` means.

> SP: yeah. it can get frustrating

> GM: It really takes like 2 seconds to just think of an appropriate name for the variable. I usually prefix with an _ just so that it sticks out a bit visually: _userModel, _slideViewer, _dialogWidget. Its not perfect but its a heck of a lot better than "that".

> GM: Variables should almost always be named after what they do/represent, not how they're implemented. Clearly we are breaking this guideline if we've got variables named "that" in different files having nothing to do with each other. The argument that it's a well known pattern isn't wrong, just that there's really no reason not to give it an even *more* descriptive name and not even have to think of the pattern.

> GM: So I guess my real point is name things properly, it will be better for everyone always and "that", "self" are just not great names.
