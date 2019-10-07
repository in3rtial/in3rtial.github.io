---
layout: post
title:  "std::map::operator[] and get_default"
date:   2019-10-06
categories: cpp utils
---

When you get a value from an std::map, a few things to remember:
- maintain const-correctness
- use [std::map::at](http://www.cplusplus.com/reference/map/map/at)
- implement a method with default (or optional) return value

## Beware of std::map[]

The std::map subscript operator ([std::map::operator[]](https://www.cplusplus.com/reference/map/map/operator[])) is tricky.
In the event that the key is not found, a new value is insterted and a reference to it is returned.
The [std::map::at](http://www.cplusplus.com/reference/map/map/at) is a good alternative that will throw when the value is not present.
Another alternative is to use a get-with-default operator. 


## Getting a default value from C++'s std::map

{% highlight c++ %}

    template <class Map, typename Key>
    typename Map::mapped_type get_default(
        const Map& map,
        const Key& key,
        const typename Map::mapped_type& dflt)
    {
        // do not optimize to return by reference, dflt would not work properly
        auto pos = map.find(key);
        return (pos != map.end()) ? (pos->second) : deflt);
    }


{% endhighlight %}

## References
- Louis Brandy's talk on the issue with the solution presented heres
    <iframe width="560" height="315" src="https://www.youtube.com/embed/lkgszkPnV8g?start=423" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- Folly's get_default implementation [Folly's get_default](https://github.com/facebook/folly/blob/a6e73f9283062c417f1390c0d738bc779581d6b8/folly/MapUtil.h#L31)
