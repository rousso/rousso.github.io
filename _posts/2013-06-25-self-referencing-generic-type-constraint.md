---
layout: single
title: Self-referencing Generic Type Constraint
date:   2013-06-25 12:00:00 +0100
categories: C#
header:
  overlay_image: /assets/images/andrew-buchanan-1384449-unsplash.jpg
  overlay_filter: 0.5
  caption: Photo by Andrew Buchanan on Unsplash
  show_overlay_excerpt: false
---
Here is a simple and nice piece of code where I found the use of a [self-referencing generic type constraint](http://en.wikipedia.org/wiki/Curiously_recurring_template_pattern) useful.

{% highlight csharp %}public interface IHierarchical<T> where T : IHierarchical<T>
{
  T Parent { get; set; }
  IList<T> Children { get; }
}
{% endhighlight %}  


This interface is used to represent a hierarchy. The generic type parameter T is constrained to IHierarchical<T>. Although at first glance it might seem like a recursion, of course, it isn't. It just requires the type argument to be implementing the same interface.  

Looking around for other guys that use this pattern, I found this [post by Eric Lippert](http://blogs.msdn.com/b/ericlippert/archive/2011/02/03/curiouser-and-curiouser.aspx) who more or less discourages it. Although I agree with him that this pattern can be confusing if not used properly, I still think it can be quite elegant and expressive if used in suitable context.  

I think this hierarchy example is a good fit for the pattern.  

{: .notice}
_Side note on the actual use of this interface:_  
_As I said, I use this to represent a hierarchy. In my code this interface is actually implemented by several classes that participate in several hierarchies. The use of the Parent pointer seems redundant, but in my case it is not as the classes that implement this interface are actually SharePoint content types, so the Children property is actually a multi-lookup column in SharePoint and the Parent pointer is the column at the other end of the relationship. Both values are populated by executing a single Linq2SharePoint query._
