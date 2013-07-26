This talk is a write-up of a lightning talk I gave at [BarcampNola 6](http://barcampnola.com/).

RequireJs is an awesome module loader that compiles with the AMD standard and in most cases the CommonJs standard as well. For my money, a lot of the "structure" that frameworks like backbone.js provide can also come out of AMD's much simpler, module-based approach.

But enough with the flame-bait, let's talk about stuff that's indisputable. The RequireJs documentation is comprehensive, informative, and nearly incomprehensible as to how to actually set stuff up.

In my experience, this is because the documentation gives bad - or at least misleading - advice. At time of writing, here's what the documentation recommends for bootstrapping your application recommends:

     <script data-main="scripts/app" src="scripts/require.js"></script>

That is to say *"load require and then use it to load scripts/app.js"* It is virtually identical therefore to

     <script src="scripts/require.js"></script>
     <script>
         require('scripts/app');
     </script>

This is the part that most people don't understand and where a lot of people get in trouble. `data-main` is just a facade for a very simple `require()` invocation, its not the **only** way to do things.

To be sure, if you are building a truly single-page application this is not an issue. Go ahead and use `data-main`. In my experience however, 
