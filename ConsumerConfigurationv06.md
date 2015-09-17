# Configuring OAuth.net Consumer Applications in v0.6 #

In version 0.6 the method of loading the OAuth.net components changed from a hard reliance  on the [Castle Windsor](http://castleproject.org/container/) inversion of control (IoC) container to use the [Common Service Locator](http://www.codeplex.com/CommonServiceLocator).  The decision was made to make this breaking change as it was recognised that users may already be using different IoC implementations within their existing applications to which they are adding an OAuth layer, or may not want to use the Castle Windsor implementation.

For more information about the Common Service Locator and choosing a service location framework, see ServiceLocatorFrameworks.

The following documentation will assume that you will be using the Castle Windsor IoC implementation.  If you intend to use an alternative IoC the examples will need to be changed to follow you IoC configuration requirements.


# Configuring OAuth.net Consumer Applications #

When writing a consumer application using OAuth.net, you need to specify which components to use for certain services. You do this through the IoC config mechanism.  Castle supports your application's **Web.config** or **App.config** file and the examples given here are for loading through these files.   Please refer to the Windsor documentation on how to load the configuration from an alternative source.

The OAuth.Net.Components assembly contains some default implementations of these services, but you may need to create your own. In both cases, configuration happens in the same way.

For consumer applications, you must provide implementations of:

  * Signing providers to handle signing for each signature method you use (implementing _ISigningProvider_)
  * A nonce provider to generate unique values for each OAuth request (implementing _INonceProvider_)

There may be more than one component for the _ISigningProvider_ service (for multiple signature methods). The signature method implemented by the signing provider is stored in the component key.

For the other services, only one component will be used, usually the first registered.


# Specifying The Microsoft Common Service Locator #

For the components to be loaded through the Microsoft Common Service Locator during the application start up you need to provide to the ServiceLocator an instance of the IoC container.

The following code specifies the Windsor container so that it loads the config from the web.config.  It specifies that the configuration will be found in a section called oauth.net.components.

```
    public class Global : HttpApplication
    {
        public override void Init()
        {
            IServiceLocator injector =
                new WindsorServiceLocator(
                    new WindsorContainer(
                        new XmlInterpreter(
                            new ConfigResource("oauth.net.components"))));

            ServiceLocator.SetLocatorProvider(() => injector);
        }
    }
```

This section of code was from the global.aspx file of a web application.

# Registering a Config Section #
Config sections are registered in the `<configSections`> element, which must be the first child of the `<configuration`> element, before any other elements.

```
<configuration>
        <configSections>
		<section name="oauth.net.components" type="Castle.Windsor.Configuration.AppDomain.CastleSectionHandler, Castle.Windsor"/>
	 </configSections>

  // etc.
</configuration>
```

This example is from a web.config.

## Registering components ##

Once you have registered a config section, you should add that config section to your config file as a child of the `<configuration`> element. The components will be specified inside a `<components>` element:

```
<configuration>
  <configSections>
    <section name="oauth.net.consumer" type="OAuth.Net.Consumer.ConfigSectionHandler, OAuth.Net.Consumer" />
  </configSections>

  <oauth.net.consumer>
    <components>
      // components ...
    </components>
  </oauth.net.consumer>
</configuration>
```

Each component has a type, a service, and an id. The type is the fully-qualified .NET type name (`"type, assembly"`). The service is the fully-qualified name of the interface the component implements. The id must be unique within the config section. Its value is only important for signing providers.

```
<component id="blah"
  service="OAuth.Net.Common.INonceProvider, OAuth.Net.Common"
  type="OAuth.Net.Components.GuidNonceProvider, OAuth.Net.Components"/>
```

For signing providers, the id should be the signature method string prefixed with `"signing.provider:"`

```
<!-- Signing provider for HMAC-SHA1 -->
<component id="signing.provider:HMAC-SHA1"
  service="OAuth.Net.Common.ISigningProvider, OAuth.Net.Common"
  type="OAuth.Net.Components.HmacSha1SigningProvider, OAuth.Net.Components" />

<!-- Signing provider for FOO-BAR -->
<component id="signing.provider:FOO-BAR"
  service="OAuth.Net.Common.ISigningProvider, OAuth.Net.Common"
  type=Acme.OAuth.Components.FooBarSigningProvider, Acme.OAuth" />
```

## Example ##

This example is taken from the Fire Eagle consumer's [Web.config](http://code.google.com/p/oauth-dot-net/source/browse/trunk/Examples/OAuth.Net.Examples.FireEagleConsumer/Web.config):

```
<configuration>
        <configSections>
		<section name="oauth.net.components" type="Castle.Windsor.Configuration.AppDomain.CastleSectionHandler, Castle.Windsor"/>
	</configSections>

	<oauth.net.components>
		<!-- Components -->
		<components>
			<!-- Signing provider for HMAC-SHA1 -->
			<component id="signing.provider:HMAC-SHA1" service="OAuth.Net.Common.ISigningProvider, OAuth.Net.Common"
				type="OAuth.Net.Components.HmacSha1SigningProvider, OAuth.Net.Components" 
				lifestyle="thread" />
			
			<!-- Nonce provider -->
			<component id="nonce.provider" service="OAuth.Net.Common.INonceProvider, OAuth.Net.Common"
						   type="OAuth.Net.Components.GuidNonceProvider, OAuth.Net.Components"/>
		</components>
        </oauth.net.consumer>
</configuration>
```