# OAuth.net 0.8.1.1 #

### 18th June 2010 ###

  * Modified to allow Consumers to be loaded into store through castle container.  Appears to throw an error if a property doesn't have a public set.
  * Changes to InMemoryConsumerStore and fleshed out example a bit more
  * Fixed issue loading comma separated list of strings.
  * Added in config settings to be able to define which roles a ConsumerRequest is granted.
  * Implemented new 2 Legged Client example to show use of Consumer Requests.
  * Moved Sign to OAuthParameters to allow easier testing and completed succesfull test of OAuthConsumerRequest
  * Fixed EchoServiceProvider to encode the echo URL correctly.  http://code.google.com/p/oauth-dot-net/issues/detail?id=22&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary
  * Changed the WorkflowHelper to handle situation where a URL redirect may have happened.  URL is now constructed based on the RawUrl and the Authority of the HttpRequest.Uri
  * Implemented patch provided through [Issue 21](https://code.google.com/p/oauth-dot-net/issues/detail?id=21) http://code.google.com/p/oauth-dot-net/issues/detail?id=21&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary
  * Overrode OAuthRequestException.ToString to contain additional data
  * Fixed cloning of AdditionalParameters
  * Fix for doubled up query string arguments when fetching a protected resource URI with query string arguments
  * Added request state store for managing the state of consumer requests;
  * Added AspNetOAuthRequest for configuring consumer requests for a typical ASP.NET environment;Updated example apps to use new APIs
  * Changed how the body of a response handles text sent down.  If it isn't parse able into a name value collection then the full content is added into as an OAuthRequestExtensionParameters.Problem
  * Removed OAuthService.CallbackUrl and replaced it with OAuthRequest.CallbackUrl
  * Removed SP-specific stuff from IToken (moved to IIssuedToken);
  * Moved OAuthToken out of Consumer into Common;
  * Changed OAuthRequest to use RequestTokenVerifier property properly;
  * Updated examples to set RequestTokenVerifier properly
  * Added NonceProvider property to ServiceProviderContext and re-named MD5HashVerifierProvider to MD5HashVerificationProvider. See http://code.google.com/p/oauth-dot-net/issues/detail?id=19&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary


# OAuth.net 0.7.1.0 #

### 5th August 2009 ###
**Breaking Changes**

The Consumer class has been modified to now accept a new EndPoint for the access and request token endpoint urls, as well as the protected resource url.  This allows you to provide different HTTP methods for each endpoint.  Overloaded constructors are provided to maintain backwards compatability.

**Non-breaking changes**

  * Changes to the IRequestIDValidator to include the token in the method
  * Added additional parse method to OAuthParameters to allow a name value collection to be directly parsed
  * Added new OAuthRequestException and modified the OAuthPipelineModule to catch uncaught exceptions and stream them correctly to the client
  * Added IsOAuthRequest property to OAuthRequestcontext to be able to deetermine in the Service Provider if the context is valid.
  * Fixed error where AuthHeader Params were not being Rfc2986 decoded
  * Fixed passing of token to consumer callback store
  * Changed OAuthRequestContext to return the RequestToken from the Accesstoken if it has not been set previously
  * Fixed Issue [17|http://code.google.com/p/oauth-dot-net/issues/detail?id=17]
  * Fixed issues with equality implementations [18|http://code.google.com/p/oauth-dot-net/issues/detail?id=18](Issue.md)
  * Changed OAuthRequest to stop the workflow if no verifier is present when getting the AccessToken.


# OAuth.net 0.7.0.0 #

### 18th June 2009 ###
**Breaking Changes**

The Consumer and Service Provider classes have both been updated to implement the [OAuth Core 1.0 Rev A specification](http://oauth.net/core/1.0a).  The implementation will not support OAuth Core 1.0 requests.

Consumer:

  * The consumer requests classes now support HTTP GET, POST, PUT and DELETE methods for the resource calls.



# OAuth.net 0.6.0.0 #

### 9th April 2009 ###
**Breaking Changes**

The method of loading components was changed from a hard reliance on the Castle Windsor IoC to implement the Microsoft Common Service Locator instead.  This is a breaking change to the configuration of your applications and application initialization.

Consumer:

  * OAuthService no longer takes the config section to load Castle from;
    * instead uses ServiceLocator.Current (overloads allow you to specify a different ServiceLocatorProvider)

  * Applications should set ServiceLocator.Current on startup
    * i.e. in Global.asax as per example apps

  * Custom config section no longer used;
    * use whatever registration mechanism your ServiceLocator supports.
    * For Windsor, this just means changing the section type (and optionally name).
    * Examples use Windsor with section name “oauth.net.components”

Service Provider:

  * ServiceProviderContext: uses ServiceLocator.Currrent rather than ServiceLocator property

  * Applications must register ServiceLocator.Current on startup
    * i.e. in Global.asax as per example apps

  * Windsor config section called “oauth.net.serviceprovider” no longer used (conflicting breaking change).
    * Use whatever registration mechanism your ServiceLocator supports
    * Examples use Windsor with section name “oauth.net.components”

  * ComponentsSection attribute removed from settings section as this is no longer the registration mechanism

  * Settings section renamed from "OAuthServiceProvider" to "oauth.net.serviceprovider" (to be consistent with framework)

Source:
  * [Download OAuthNet-source-0.6.0.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-source-0.6.0.0.zip)
  * [Browse tags/0.6.0.0-release in repository](http://code.google.com/p/oauth-dot-net/source/browse/#svn/tags/0.6.0.0-release)
  * `svn checkout http://oauth-dot-net.googlecode.com/svn/tags/0.6.0.0-release/ oauth-0.6.0.0-release`

Binaries:
  * [Download OAuthNet-Release-0.6.0.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-Release-0.6.0.0.zip&)

# OAuth.net 0.5.2.0 #

### 12th March 2009 ###
  * Fixed [#9](http://code.google.com/p/oauth-dot-net/issues/detail?id=9&can=1&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary): Incorrect Url Decoding of parameters.
  * Fixed [#11](http://code.google.com/p/oauth-dot-net/issues/detail?id=11&can=1&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary): Broken CheckSignature in PlainTextSigningProvider
  * Fixed [#12](http://code.google.com/p/oauth-dot-net/issues/detail?id=12&can=1&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary): Issue with ASP.NET AJAX UpdatePanel causing headers to create a null key when parsed.
  * Fixed [#13](http://code.google.com/p/oauth-dot-net/issues/detail?id=13&can=1&colspec=ID%20Type%20Status%20Priority%20Owner%20Assembly%20Summary): InMemoryStoreTokenStore bug returning matching tokens.

Source:
  * [Download OAuthNet-source-0.5.2.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-source-0.5.2.0.zip)
  * [Browse tags/0.5.2.0-release in repository](http://code.google.com/p/oauth-dot-net/source/browse/#svn/tags/0.5.2.0-release)
  * `svn checkout http://oauth-dot-net.googlecode.com/svn/tags/0.5.2.0-release/ oauth-0.5.2.0-release`

Binaries:
  * [Download OAuthNet-Release-0.5.2.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-Release-0.5.2.0.zip&)

# XRDS-Simple 0.6.0.0 #

### 12th Januray 2009 ###
  * Initial release of XRDS-Simple.net library for OAuth discovery

Source:
  * [Download XRDS-Simple.net Source Code 0.6.0.0](http://oauth-dot-net.googlecode.com/files/XRDSSimpleNet-source-0.6.0.0.zip&)

Binaries:
  * [Download XRDS-Simple.net Release 0.6.0.0](http://oauth-dot-net.googlecode.com/files/XRDSSimpleNet-Release-0.6.0.0.zip&)



---


# OAuth.net 0.5.1.0 #

### 26th September 2008 ###
  * Fixed [#8](http://code.google.com/p/oauth-dot-net/issues/detail?id=8): could not build source without password for signing key. Everyone should now be able to view and build the source (unsigned).

Source:
  * [Download OAuthNet-source-0.5.1.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-source-0.5.1.0.zip)
  * [Browse tags/0.5.1.0-release in repository](http://code.google.com/p/oauth-dot-net/source/browse/#svn/tags/0.5.1.0-release)
  * `svn checkout http://oauth-dot-net.googlecode.com/svn/tags/0.5.1.0-release/ oauth-0.5.1.0-release`

Binaries:
  * [Download OAuthNet-Release-0.5.1.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-Release-0.5.1.0.zip&)


---


# OAuth.net 0.5.0.0 #

### 5th September 2008 ###
  * Initial release
  * Support for consumers and service providers

Source:
  * [Download OAuthNet-source-0.5.0.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-source-0.5.0.0.zip)
  * [Browse tags/0.5.0.0-release in repository](http://code.google.com/p/oauth-dot-net/source/browse/#svn/tags/0.5.0.0-release)
  * `svn checkout http://oauth-dot-net.googlecode.com/svn/tags/0.5.0.0-release/ oauth-0.5.0.0-release`

Binaries:
  * [Download OAuthNet-Release-0.5.0.0.zip](http://code.google.com/p/oauth-dot-net/downloads/detail?name=OAuthNet-Release-0.5.0.0.zip&)