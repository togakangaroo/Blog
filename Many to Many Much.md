Since I am one of the senior developers at [Surge Consulting](http://www.surgeforward.com), and since many of our projects use an admittedly poorly documented [custom ORM](https://bitbucket.org/ufx/nen), I frequently field questions as to how to achieve certain things.

By far, the most popular question is "how do I do a many-to-many relationship in Nen?" There are several answers to this question but the first thing I almost always ask is why the developer feels the need for a many-to-many. The conversation usually follows along the same lines and applies equally to Nen, EntityFramework, NHibernate, or any data-access strategy.

> **Joe Developer**: Well I have two entities that reference each other. This is a project management application. Programmers work on Projects and Projects can have multiple Programmers so I need to set up a many to many.

> **Me**: That makes sense, but does that need to be implemented as a many-to-many?

> **Joe**: What do you mean? That's what it is, how else can you implement it?

And there is the issue, people rarely trully consider all of the options. A many-to-many is not *the* option, it's just the most *normalized* one. Normalization is good, it should be the default. However, in the case where you are feeling the need for a many-to-many I think it requires a second look.

Before we talk about the options, let's consider the problems a many-to-many relationship poses. This is not to say such a relationship is bad, like I said, normalization is good, but it does have its negatives.

You either need lazy loading or you trigger a select n+1 query
Deletions require a decision on cascading or not
Database migrations are more awkward
Can be more difficult to map
Strongly coupling two object hierarchies
I'm working with projects, but I have to think about programmers

Alternatives
============

Different bounded contexts with no relationship.
  Manually enforce consistnecy.
  Eventual consistency from another component.
Different bounded contexts with an unenforced FK relationship
Serialization into a sting field
Explicit join object

Considerations
==============
Do you need JUST the relationship? Is there data associated with the relationship itself?
What contexts will this be used in.
