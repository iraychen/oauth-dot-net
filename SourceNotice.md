The current source trunk is under active development, and is likely to be unstable.  The latest stable release is 0.8.1.1 available from the [download section](http://code.google.com/p/oauth-dot-net/downloads/list) (or browse the [0.8.1.1 release source](http://code.google.com/p/oauth-dot-net/source/browse/#svn/tags/0.8.1.1-release)).

[Continue to browse source...](http://code.google.com/p/oauth-dot-net/source/browse/)

# New On The Trunk #

July 2010 -
  * Work on 1.0a spec is now frozen apart from any further bug fixes
  * Restructure of the project and initial work on OAuth 2

March 2010 -

  * Implementation to support Consumer Requests (2-legged OAuth) in the Consumer and Service Provider are now in the trunk.

  * Example code of making Consumer Requests has now been added.

Dec 2009 -

  * Work to streamline API for consumers (ongoing, disruptive)

Sep 2009 -

  * Implementing changes for the OAuth Core 1.0 Rev A spec.

14th May 2009 - Changes made to consumer classes commited.

18th June 2009 - Changes made to service provider classes commited.

  * Implementing changes to support RESTful end points and allow different HttpMethod for the protected resource.

9th May 2009 - Changes made to support GET, POST, PUT, and DELETE.  Additional HttpMethod property on the OAuthRequest class provides functionality to change the http method for requesting the protected resource.

  * Implementing changes to allow different HttpMethods for each oauth endpoint.
18th June 2009 - Commited changes to Consumer classes to implement new EndPoint class.  You are now able to specify a different HTTP method for the access token, request token and protected resource end point indepentdently.


  * Work to implement the [Consumer Request Extension](http://oauth.googlecode.com/svn/spec/ext/consumer_request/1.0/drafts/2/spec.html)
13th May 2009 - Currently Delayed to allow for OAuth Core 1.0 Rev A spec.