---
title: "Ein Hamcrest Entity Matcher"
categories: software_craftsmanship
author: stefan.lack
---

bla blub



* foo
* bar

{% highlight java %}
public static <T> EntityMatcher<T> matchesAllProperties(T expected) {
    return new EntityMatcher<T>(expected,MatcherType.EXCLUDE_NAMED_FIELDS);
}

{% endhighlight %}
> "zitat"
