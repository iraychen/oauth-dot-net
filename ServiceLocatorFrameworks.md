# What is Service Location? #

OAuth.net is designed with a pluggable component based architecture. The core library defines a series of **services** (interfaces) which are used, either by the consumer code, the service provider code, or both. These services are:

  * _ISigningProvider_ : provides signature computation and checking (C, SP)
  * _INonceProvider_ : generates nonces for requests (C)
  * _ITokenGenerator_ : generates new request and access tokens (SP)
  * _IRequestIdValidator_ : validates the nonce and timestamps of incoming requests (SP)
  * _IConsumerStore_ : abstracts storage and lookup of consumer records (SP)
  * _ITokenStore_ : abstracts storage and lookup of request and access tokens (SP)

When one of these services is required, the core libraries use a service locator to locate a concrete implementation (a component). This is variously known as service location. This is related to the concepts of dependency injection (DI) and inversion of control (IoC). Martin Fowler has [an article](http://martinfowler.com/articles/injection.html) which explores these concepts in more detail.

The upshot of all of this is that where a service is defined _you can write your own components which customise how OAuth.net works_. By registering your components with the service locator, you instruct OAuth.net to use your implementation.

# What is the Common Service Locator? #

There are many different service location/DI/IoC frameworks (see below). While each has its own specialisations and features, they all offer one common denominator: service location. The [Common Service Locator](http://www.codeplex.com/CommonServiceLocator) is a combined effort from those involved in the various libraries and Microsoft Patterns and Practices to provide a common mechanism for service location, whichever library you use (you can even roll your own!). For technical details see the [Codeplex website](http://www.codeplex.com/CommonServiceLocatorCSL).

OAuth.net uses the CSL to talk to whichever service locator you use, and there are a great many to choose from. The CSL is published under the [MS-PL open source license](http://commonservicelocator.codeplex.com/license).

# What Service Location frameworks exist for .NET? #

Here is a list of some of the service location/DI/IoC frameworks available today for .NET:

  * [Castle Windsor](http://castleproject.org/container/) ([CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=Castle%20Windsor%20Adapter)) [Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0.html) license
  * [Spring.NET](http://www.springframework.net/) ([CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=Spring%20.NET%20Adapter)) [Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0.html) license
  * [Microsoft P&P Unity](http://www.codeplex.com/unity) ([CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=Unity%20Adapter)) [MS-PL](http://unity.codeplex.com/license) license
  * [StructureMap](http://structuremap.sourceforge.net/) ([CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=StructureMap%20Adapter)) [Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0.html) license
  * [autofac](http://code.google.com/p/autofac/) ([CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=Autofac%20Adapter)) [MIT](http://www.opensource.org/licenses/mit-license.php) license
  * [Microsoft MEF](http://www.codeplex.com/MEF) ([CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=MEF%20Adapter)) [MS-PL](http://mef.codeplex.com/license) license

Another popular framework is [Ninject](http://ninject.org/). Ninject does not currently have a CSL adaptor, since it does not support the full CSL interface, but CSL support is [coming in V2](http://kohari.org/2009/02/25/ninject-2-reaches-beta/), currently in beta.

NB: This list is correct at the time of writing (April 2009).

See also: [List of .NET Dependency Injection Containers](http://www.hanselman.com/blog/ListOfNETDependencyInjectionContainersIOC.aspx) by Scott Hanselman and commenters

# Which framework should I use? #

If you are already using a service location framework in your application and it supports CSL, use that! If it doesn't support CSL, it may be a simple matter to [write your own CSL adaptor](http://commonservicelocator.codeplex.com/Wiki/View.aspx?title=Implementing%20the%20Interface) for it.

If you're not currently using service location/DI/IoC but want to use OAuth.net, then any of the above libraries should be suitable. OAuth.net only exercises a small subset of their features so don't worry too much about their feature sets. Windsor and StructureMap are particularly popular, and the examples in the OAuth.net source code tend to use Windsor. Please be aware of the differing licenses if you need to redistribute your source code.

Updates and feedback on this page are welcome. Please email oauth-dot-net@madgex.com