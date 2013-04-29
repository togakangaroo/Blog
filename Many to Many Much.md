Since I am one of the senior developers at [Surge Consulting](http://www.surgeforward.com), and since many of our projects use an admittedly poorly documented [custom ORM](https://bitbucket.org/ufx/nen), I frequently field questions as to how to achieve certain things.

By far, the most popular question is "how do I do a many-to-many relationship in Nen?" There are several answers to this question but the first thing I almost always ask is why the developer feels the need for a many-to-many. The conversation usually follows along the same lines and applies equally to Nen, EntityFramework, NHibernate, or any data-access strategy.

> **Developer**: Well I have two entities that reference each other. Programmers work on Projects and Projects can have multiple Programmers.
