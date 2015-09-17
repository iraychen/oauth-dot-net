# Configuring OAuth.net Consumer Applications #

When writing a consumer application using OAuth.net, you need to specify which components to use for certain services. You do this using your application's **Web.config** or **App.config** file.

The OAuth.Net.Components assembly contains some default implementations of these services, but you may need to create your own. In both cases, configuration happens in the same way.

For consumer applications, you must provide implementations of:

  * Signing providers to handle signing for each signature method you use (implementing _ISigningProvider_)
  * A nonce provider to generate unique values for each OAuth request (implementing _INonceProvider_)

There may be more than one component for the _ISigningProvider_ service (for multiple signature methods). The signature method implemented by the signing provider is stored in the component key.

For the other services, only one component will be used, usually the first registered.

## Registering a Config Section ##

To register components for your consumer application, you must first register a config section. Config sections are registered in the `<configSections`>` element, which must be the first child of the `<configuration`>  element, before any other elements.

By convention, consumer applications load components from a section called `oauth.net.consumer` of type `"OAuth.Net.Consumer.ConfigSectionHandler, OAuth.Net.Consumer"`.

If the consumer uses more than one service, it may specify multple, independent config sections. In this case, the sections can be given any valid XML name, which is supplied when constructing the `OAuthService` in code.

For example:

```
<configuration>
  <configSections>
    <section name="oauth.net.consumer" type="OAuth.Net.Consumer.ConfigSectionHandler, OAuth.Net.Consumer" />
  </configSections>

  // etc.
</configuration>
```

Or:

```
<configuration>
  <configSections>
    <section name="my-components" type="OAuth.Net.Consumer.ConfigSectionHandler, OAuth.Net.Consumer" />
  </configSections>

  // etc.
</configuration>
```

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
    <section name="oauth.net.consumer" type="OAuth.Net.Consumer.ConfigSectionHandler, OAuth.Net.Consumer" />
  </configSections>

  <!-- OAuth consumer settings -->
  <oauth.net.consumer>
    <!-- Components -->
    <components>

      <!-- Signing provider for HMAC-SHA1 -->
      <component id="signing.provider:HMAC-SHA1" 
        service="OAuth.Net.Common.ISigningProvider, OAuth.Net.Common"
        type="OAuth.Net.Components.HmacSha1SigningProvider, OAuth.Net.Components" />

      <!-- Nonce provider -->
      <component id="nonce.provider"
        service="OAuth.Net.Common.INonceProvider, OAuth.Net.Common" 
        type="OAuth.Net.Components.GuidNonceProvider, OAuth.Net.Components"/>

    </components>
  </oauth.net.consumer>
</configuration>
```

## Using Custom Config Sections ##

If your config section has a non-standard name, you must use one of the overrides for `OAuthService.Create` that takes `configSection` as a parameter:

```
// Config section is <my-components>
var service = OAuthService.Create(
  /* request token uri */,
  /* authorize uri */,
  /* access token uri */,
  /* consumer */,
  "my-components");
```