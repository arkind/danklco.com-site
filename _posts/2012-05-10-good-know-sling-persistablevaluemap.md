---
layout: post
title: "Good to Know: The Sling PersistableValueMap"
original: http://labs.sixdimensions.com/blog/dklco/2012-05-10/good-know-sling-persistablevaluemap
summary: Learn about the PersistableValueMap which makes it possible to save values to the JCR datastore through the Sling API.
tags: [Adobe CQ, Apache Sling]
---

The Sling [ValueMap][1] makes retrieving properties from CQ easy and removes a lot of the error-prone code you have to use when using the JCR APIs, however it does not allow setting of properties.  This unfortunately results in developers using the Sling ValueMap to retrieve properties and JCR Nodes and Properties to set properties. 

This doesn't have to be the case though!  Sling also provides an interface which allows for retrieving properties like a ValueMap and an easy faculty for setting properties as well.  The [PersistableValueMap][2] differs from the ValueMap only in it's put method does not throw an exception and it offers two more methods.  The [reset][3] method, which resets any values set in the current session and the [save][4] method, which persists any values set in the current session.

Like the ValueMap, any Sling [Resource][5] can be adapted to a PersistableValueMap, to retrieve a PersistableValueMap, simply call:

    PersistableValueMap propertes = aResource.adaptTo(PersistableValueMap.class);

Once you have an instance of the PersistableValueMap, you can use it like a ValueMap

    String value = propertes.get("key",String.class);

Or use it to persist values to CQ:

    propertes.put("key","value");properties.save();

 [1]: http://sling.apache.org/apidocs/sling6/org/apache/sling/api/resource/ValueMap.html
 [2]: http://sling.apache.org/apidocs/sling6/org/apache/sling/api/resource/PersistableValueMap.html
 [3]: http://sling.apache.org/apidocs/sling6/org/apache/sling/api/resource/PersistableValueMap.html#reset%28%29
 [4]: http://sling.apache.org/apidocs/sling6/org/apache/sling/api/resource/PersistableValueMap.html#save%28%29
 [5]: http://sling.apache.org/apidocs/sling6/org/apache/sling/api/resource/Resource.html  