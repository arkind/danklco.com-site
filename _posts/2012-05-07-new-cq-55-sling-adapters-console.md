---
layout: post
title: "New in CQ 5.5: Sling Adapters Console"
original: http://labs.sixdimensions.com/blog/dklco/2012-05-07/new-cq-55-sling-adapters-console
summary: Introduction to the new Sling Adapters Console, available in Adobe CQ5.
tags: [Adobe CQ, CQ 5.5, Apache Sling]
---

Adobe CQ 5.5 has a new, long overdue feature in the Felix Console.  The Sling Adapters Console lists all of the available Sling Adapter Factories as well as their Adapter Classes, Adaptable Classes, condition information and Providing bundle.  This information can be invaluable for developers to determine what classes can be adapted and what they can be adapted into as well as checking the functioning of custom adapters.  Developers can access this information by logging on to {CQ_SERVER}/system/console/bundles or selecting the Sling Adapters tab on the Felix Console.

 ![Sling Adapter Console][1]

The Sling Adapters Console does rely on additional metadata to retrieve the list of Adapters, so you may have to add some additional annotations to get existing custom adaptors to display on this list.

 To get your custom Adapter to display in the Sling Adapters Console, it should implement the [org.apache.sling.api.adapter.AdapterFactory][2] interface and has the following Felix properties:

*   adaptables - Classes which this adapter can adapt
*   adapters - Classes to which the adaptables can be adapted
*   adapter.condition - (optional) under what conditions the adaptables can be adapted to the adapters

An example of what the annotations would look like would be:

    @Component@Service(value = AdapterFactory.class)@Properties({
        @Property(name = "adaptables", value = "org.apache.sling.api.resource.Resource"),
        @Property(name="adapter.condition", value="Any existing resource"),
        @Property(name = "service.vendor", value = "Six Dimensions"),
        @Property(name = "service.description", value = "Service for adapting Sling Resources to JSON Objects."),
        @Property(name = "adapters", value = { "org.apache.sling.commons.json.JSONObject" })
    })

This provides the metadata for an adapter that adapts Sling Resources to JSONObjects.

To read more on the genesis of the Sling Adapters Console, read the proposal discussion on Nabble:

[apache-sling.73963.n3.nabble.com/PROPOSAL-add-Adapter-Metadata-services-td3501542.html][3]

 [1]: /images/posts/2012-05-07-new-cq-55-sling-adapters-console/SlingAdaptersConsole.jpg "Sling Adapter Console"
 [2]: http://sling.apache.org/apidocs/sling6/org/apache/sling/api/adapter/AdapterFactory.html "Sling Adapter Factory Interface"
 [3]: http://apache-sling.73963.n3.nabble.com/PROPOSAL-add-Adapter-Metadata-services-td3501542.html  