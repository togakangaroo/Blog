This talk is a write-up of a lightning talk I gave at [BarcampNola 6](http://barcampnola.com/). [See here for a sample project demonstrating this techniqe](https://github.com/togakangaroo/setting-up-requirejs).

[RequireJs](http://requirejs.org/) is an awesome module loader that complies with the AMD standard and in most cases the CommonJs standard as well. For my money, a lot of the "structure" that frameworks like backbone.js provide can also come out of AMD's much simpler, module-based approach.

But enough with the flame-bait, let's talk about stuff that's indisputable. The RequireJs documentation is comprehensive, informative, and nearly incomprehensible as to how to actually set stuff up.

In my experience, this is because the it gives bad - or at least misleading - advice. At time of writing, here's what the documentation for bootstrapping your application recommends:

     <script data-main="scripts/app" src="scripts/require.js"></script>

That is to say *"load require and then use it to load scripts/app.js"* It is virtually identical therefore to

     <script src="scripts/require.js"></script>
     <script>
         require(['scripts/app']);
     </script>

This is the part that most people don't understand and where a lot of people get in trouble. `data-main` is just a facade for a very simple `require()` invocation. And its certainly not the **only** way to do things.

# The Problem

To be sure, if you are building a truly single-page application this is not an issue. Go ahead and use `data-main`. In my experience however, very few SPAs truly have only one page; there are frequently different "areas". For example it is common for the transition between administrator and a user-facing areas to be full page requests with entirely different resource sets for each.

The problem is that while the resources and modules used might vary, the configuration (the `requirejs.config()` call required to configure what directory scripts are in, any shims, path aliases, etc) is likely exactly the same for the two. 

Another issue emerges when using the [RequireJs optimizer](http://requirejs.org/docs/optimization.html). The way this works is that during deployment, you point the optimizer at a javascript file containing a RequireJs module that starts your application. It will then walk the dependency tree concatenating, ordering, and optionally minifying all referenced modules. The result is your entire application in a single bootstrapping javascript file thereby minimizing both HTTP request count and size.

Of course once you have multiple pages, walking the dependencies for each will generate bundles which contain overlapping code for common libraries such as `jQuery`. This eats up many of those same bytes that we saved by minifying to begin with!

# The Solution

The solution is to differentiate *global* scripts such as `jQuery` and your `requirejs.config()` from your page-specific ones. Ultimately your pages in deployment will be producing three javascript HTTP requests
 
1. The RequireJs library itself
2. Configuration and modules used globally across all areas of your application
3. Modules required within the specific area

The first two can safely be cached by the browser and will likely change relatively little.

The global modules bootstrapper would look something like this:

    requirejs.config({
         baseUrl: '/scripts'
        ,shim: {
            underscore: {exports: '_'}
        }
        ,paths: {
            jquery: 'jquery-2.0.0'
        }
    });
    require(['jquery', 'underscore', 'globalHelpers']); 

the require statement at the end does double-duty here. When running the application unoptimized it starts requirejs on loading these global files immediately so they are cashed by the time other parts of the application request them. When deploying, in the meantime, the configuration will be bundled along with these globally used scripts into a single file. These scripts can then be excluded from the bundle for the page itself (see addendum for more on this).

With this in place, your script includes end up looking like this

     <script src="scripts/require.js"></script>
     <script>
         require(['scripts/global'], function(){
              require(['clientApp']);  //or adminApp for the admin area, etc.
         });
     </script>

It is a bit more code, but now everything can be grouped easily into three logical request chains and - after being optimized - three logical bundles.

Nothing else needs to change.

----------------------------------------

# Addendum 

I should say that I'm not fully satisfied with all aspects of this system. Specifically, the global bootstrapper above and the build configuration file used by the optimizer are very similar, but different enough that one cannot be substituted for the other. For example, given the above, the build configuration will probably look something like this

    {
         baseUrl: '../app/scripts'                //where to find all our scripts
        ,dir: '../build/scripts'                      //where to output scripts

        ,optimize: 'uglify2'
        ,generateSourceMaps: true

        ,shim: {
            underscore: {exports: '_'}
        }
        ,paths: {
            jquery: 'jquery-2.0.0'
        }

        ,modules: [  //what should actually be built into bundles
            {
                name: 'global'               //the global bundle
            }
            ,{
                 name: 'client'               //the client page bundle
                ,exclude: ['jquery', 'underscore', 'globalHelpers']
            }
            ,{
                 name: 'admin'               //the admin page bundle
                ,exclude: ['jquery', 'underscore', 'globalHelpers']
            }
        ]
    }

[This example build configuration file is useful for learning all the optimizer configuration properties](https://github.com/jrburke/r.js/blob/master/build/example.build.js).

Note the many similarities! The shims and paths are exactly the same. But while `global.js` is a javascript file, this one is passed through a JSON parser. In addition, while the list of globals is explicitly included in `globals.js`, in the build configuration it is explicitly *excluded* from all other modules.

This is all sorts of awkward. I solved the issue with a custom template written for my build system that generates both files, but that is an article for another day.
