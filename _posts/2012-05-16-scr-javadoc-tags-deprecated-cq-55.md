---
layout: post
title: "SCR JavaDoc Tags Deprecated in CQ 5.5"
original: http://labs.sixdimensions.com/blog/dklco/2012-05-16/scr-javadoc-tags-deprecated-cq-55
summary: In CQ 5.5, by default you cannot compile code with CRXDE, learn about how to fix this.
tags: [Adobe CQ, CQ 5.5, Apache Felix, CRXDE]
---

Teams migrating to Adobe CQ 5.5 have one more thing to check during the upgrade.&nbsp; In CQ 5.5, the [SCR JavaDoc tags][1] are deprecated, because of this, you can no longer create bundles in CRXDE or CRXDE Lite when the code in the bundles contains SCR JavaDoc tags.&nbsp;&nbsp;

When Adobe ported CRX over to run inside of the OSGi Container, they created a bundle for the CRXDE functionality.&nbsp; This includes the remote compiling and bundle builder used by CRXDE and CRXDELite.&nbsp; In the bundle they set the bundle builder to create bundles using 'strict mode', this mode triggers an error when it encounters the deprecated SCR JavaDoc tags and prevents the building of the SCR Descriptor XML.&nbsp; A JAR will be created, however it will not have the OSGi metadata needed to register services in OSGi.&nbsp;

### How can I tell if my project is affected?

Neither CRXDE nor CRXDE Lite tell you that the SCR JavaDoc tags are causing the issue when you try to build the bundle.&nbsp; Both indicate an error has occurred, but do not say what caused the error.&nbsp;

In CRXDE Lite you will see a message "SCR Descriptor parsing had failures (see log)".&nbsp; It does tell you which classes had the deprecated javadoc tags however, which will be useful.

![CRXDE Lite Deprecated Message][2]

CRXDE will report that the build failed and tell you to check the Remote Build view for details on the errors.&nbsp; Unfortunately, it will not actually have any error information availabe in the Remote Build view.

![CRXDE Build Failure Popup][3]

Finally, if you look in the logs you will see messages like the below:

    16.05.2012 20:35:03.789 *WARN* [127.0.0.1 [1337214903288] POST /libs/crxde/build HTTP/1.1] com.day.crx.ide.BundleBuilder Class [...]Process is using deprecated javadoc tags&nbsp; at jcr:/_/crx.default/apps/public/src/[...]Bundle/src/[...]Process.java:39
	16.05.2012 20:35:03.789 *WARN* [127.0.0.1 [1337214903288] POST /libs/crxde/build HTTP/1.1] com.day.crx.ide.BundleBuilder Class [...]Servlet is using deprecated javadoc tags&nbsp; at jcr:/_/crx.default/apps/public/src/[...]Bundle/src/main/java/[...]Servlet.java:39
	16.05.2012 20:35:03.789 *ERROR* [127.0.0.1 [1337214903288] POST /libs/crxde/build HTTP/1.1] com.day.crx.ide.BundleBuilder It is highly recommended to fix these problems, as future versions might not support these features anymore.
	16.05.2012 20:35:03.789 *WARN* [127.0.0.1 [1337214903288] POST /libs/crxde/build HTTP/1.1] com.day.crx.ide.BundleBuilder SCR Descriptor parsing had failures (see log) at :0`

### How Can I Fix My Project?

The only way to fix your project is to replace all of the SCR JavaDoc Tags with the new [SCR Annotations][4].&nbsp; The fields and values will be the same or similar, only the syntax changes.&nbsp;

Once you do this, download and install the [Felix SCR Annotations Bundle][5] into your OSGi console.&nbsp; Without this bundle, you will not be able to compile your project code within CRXDE or CRXDE Lite as the Felix SCR Annotations packages are not currently exported by any bundle in the Generally Available release of CQ 5.5.

 [1]: http://felix.apache.org/site/scr-javadoc-tags.html "Apache Felix Documentation on SCR JavaDoc Tags"
 [2]: /images/posts/2012-05-16-scr-javadoc-tags-deprecated-cq-55/crxdelite-deprecated-message.png "CRXDE Lite Deprecated Message"
 [3]: /images/posts/2012-05-16-scr-javadoc-tags-deprecated-cq-55/crxde-error-message.png "CRXDE Build Failure Popup"
 [4]: http://felix.apache.org/site/scr-annotations.html "Apache Felix SCR Annotations Documentation"
 [5]: http://www.6dlabs.com/content/felix-scr-annotations-bundle "Felix SCR Annotations Bundle"  