---
title: "Support Configuration With Namespace"
date: 2023-07-26T14:16:08+09:00
draft: false
series:
  - Spring Data Meilisearch
categories:
  - Project
tags:
  - Java
  - Spring
  - Meilisearch
  - Library
summary: >
 How to support namespace-based configuration in a Spring library
---

> Note: This English page scaffold was auto-generated. Full manual translation will follow.


While working on the [Spring Data Meilisearch](https://github.com/junghoon-vans/spring-data-meilisearch) project, we had to provide the ability to register `Meilisearch client` as a Spring bean.
There are two main ways to register a Spring bean.

- Configuration based on `XML namespace`
- Settings based on `Java Annotation`

In this post, we will cover how to support ‘XML-based configuration’ in the Spring library.

> If you want to know how to support `annotation-based configuration`, please refer to [next post](/posts/project/spring-data-meilisearch/support-configuration-with-annotation).

## How to set up a namespace

By defining the following in `namespace.xml`, you can create a Meilisearch client and register it as a Spring bean.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:meilisearch="http://www.vanslog.io/spring/data/meilisearch"
 xsi:schemaLocation="http://www.vanslog.io/spring/data/meilisearch http://www.vanslog.io/spring/data/meilisearch/spring-meilisearch-1.0.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

 <meilisearch:meilisearch-client id="meilisearchClient"/>
</beans>
```

## Implementation method

To support XML-based configuration, the following implementation is required:

- `XSD definition` for namespace-based configuration
- `Register namespace` through `NamespaceHandler`
- `Attribute parsing` required for bean registration through `BeanDefinitionParser`
- Meilisearch `client creation and bean registration` through `FactoryBean`

### XSD definitions

XSD is a schema language that defines the structure and content of an XML document. Spring uses XSD to support configuration via XML. A file called `spring-meilisearch-1.0.xsd` was defined to support XML-based configuration in Spring Data Meilisearch.

> Since the content is long, please click the button below to check the content.

<details><summary>spring-meilisearch-1.0.xsd</summary>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"
 xmlns:beans="http://www.springframework.org/schema/beans"
 xmlns:tool="http://www.springframework.org/schema/tool"
 xmlns="http://www.vanslog.io/spring/data/meilisearch"
 targetNamespace="http://www.vanslog.io/spring/data/meilisearch"
 elementFormDefault="qualified" attributeFormDefault="unqualified">

 <xsd:import namespace="http://www.springframework.org/schema/beans"/>
 <xsd:import namespace="http://www.springframework.org/schema/tool"/>

 <xsd:element name="meilisearch-client">
 <xsd:annotation>
 <xsd:documentation/>
 <xsd:appinfo>
 <tool:assignable-to type="com.meilisearch.sdk.Client"/>
 </xsd:appinfo>
 </xsd:annotation>
 <xsd:complexType>
 <xsd:complexContent>
 <xsd:extension base="beans:identifiedType">
 <xsd:attribute name="host-url" type="xsd:string" default="http://localhost:7700">
 <xsd:annotation>
 <xsd:documentation>
 <![CDATA[The host address of the Meilisearch server. The default is http://localhost:7700.]]>
 </xsd:documentation>
 </xsd:annotation>
 </xsd:attribute>
 <xsd:attribute name="api-key" type="xsd:string">
 <xsd:annotation>
 <xsd:documentation>
 <![CDATA[The API key of the Meilisearch server.]]>
 </xsd:documentation>
 </xsd:annotation>
 </xsd:attribute>
 <xsd:attribute name="json-handler" default="GSON">
 <xsd:annotation>
 <xsd:documentation>
 <![CDATA[The enum value of java: io.vanslog.spring.data.meilisearch.config.JsonHandlerBuilder. The default is GSON.]]>
 </xsd:documentation>
 </xsd:annotation>
 <xsd:simpleType>
 <xsd:restriction base="xsd:string">
 <xsd:enumeration value="GSON">
 <xsd:annotation>
 <xsd:documentation>
 <![CDATA[Use GSON as the JSON handler.]]>
 </xsd:documentation>
 </xsd:annotation>
 </xsd:enumeration>
 <xsd:enumeration value="JACKSON">
 <xsd:annotation>
 <xsd:documentation>
 <![CDATA[Use JACKSON as the JSON handler.]]>
 </xsd:documentation>
 </xsd:annotation>
 </xsd:enumeration>
 </xsd:restriction>
 </xsd:simpleType>
 </xsd:attribute>
 <xsd:attribute name="client-agents" type="xsd:string">
 <xsd:annotation>
 <xsd:documentation>
 <![CDATA[The comma delimited string array of client agents.]]>
 </xsd:documentation>
 </xsd:annotation>
 </xsd:attribute>
 </xsd:extension>
 </xsd:complexContent>
 </xsd:complexType>
 </xsd:element>

</xsd:schema>

```
</details>

#### Elements and Attributes

The above XSD defines the `meilisearch-client` element. The element has the following properties.

- `host-url`
- Specifies the address of the Meilisearch server.
- Default is `http://localhost:7700`.
- `api-key`
- Specify the API key of the Meilisearch server.
- `json-handler`
- Specifies the library that processes JSON.
- The default is `GSON`.
- `client-agents`
- Specifies the agent of the Meilisearch client.
- The default is an empty array.

### NamespaceHandler

```java
public class MeilisearchNamespaceHandler extends NamespaceHandlerSupport {

 @Override
 public void init() {
 registerBeanDefinitionParser("meilisearch-client", new MeilisearchClientBeanDefinitionParser());
 }
}
```

The `meilisearch-client` element defined in the previous XSD was registered through `MeilisearchNamespaceHandler`. At this time, the `MeilisearchClientBeanDefinitionParser` registered together plays the role of parsing the properties required for bean registration. We will cover this in detail later.

```properties
http\://www.vanslog.io/spring/data/meilisearch=io.vanslog.spring.data.meilisearch.config.MeilisearchNamespaceHandler
```

Afterwards, we registered `MeilisearchNamespaceHandler` in the `spring.handlers` file so that Spring can use the handler.

### BeanDefinitionParser

```java
public class MeilisearchClientBeanDefinitionParser
 extends AbstractBeanDefinitionParser {

 @Override
 protected AbstractBeanDefinition parseInternal(Element element,
 ParserContext parserContext) {
 BeanDefinitionBuilder builder =
 BeanDefinitionBuilder.rootBeanDefinition(
 MeilisearchClientFactoryBean.class);
 setLocalSettings(element, builder);
 return getSourcedBeanDefinition(builder, element, parserContext);
 }

 private void setLocalSettings(Element element,
 BeanDefinitionBuilder builder) {

 Assert.hasText(element.getAttribute("api-key"),
 "The attribute 'api-key' is required.");

 builder.addPropertyValue("hostUrl", element.getAttribute("host-url"));
 builder.addPropertyValue("apiKey", element.getAttribute("api-key"));
 builder.addPropertyValue("clientAgents",
 element.getAttribute("client-agents"));

 String jsonHandlerName = element.getAttribute("json-handler");
 Assert.isTrue(JsonHandlerBuilder.contains(jsonHandlerName),
 "JsonHandler must be one of "
 + Arrays.toString(JsonHandlerBuilder.values()));

 JsonHandlerBuilder handlerBuilder =
 JsonHandlerBuilder.valueOf(jsonHandlerName.toUpperCase());
 builder.addPropertyValue("jsonHandler", handlerBuilder.build());
 }

 private AbstractBeanDefinition getSourcedBeanDefinition(
 BeanDefinitionBuilder builder, Element source,
 ParserContext context) {
 AbstractBeanDefinition definition = builder.getBeanDefinition();
 definition.setSource(context.extractSource(source));
 return definition;
 }
}
```

There are four pieces of information required to create a Meilisearch client. So, we parsed hostUrl, apiKey, clientAgents, and jsonHandler so that `MeilisearchClientFactoryBean` could create a Client object.

### FactoryBean

`MeilisearchClientFactoryBean` is a class that inherits `FactoryBean` and is responsible for creating a Meilisearch client and registering it as a `Spring Bean`.

```java
public final class MeilisearchClientFactoryBean
 implements FactoryBean<Client>, InitializingBean {

 @Nullable private String hostUrl;
 @Nullable private String apiKey;
 @Nullable private JsonHandler jsonHandler;
 private String[] clientAgents;
 @Nullable private Client client;

 private MeilisearchClientFactoryBean() {
 this.clientAgents = new String[0];
 }

 @Override
 public Client getObject() {
 return client;
 }

 @Override
 public Class<? extends Client> getObjectType() {
 return Client.class;
 }

 @Override
 public void afterPropertiesSet() throws Exception {
 Config config = new Config(hostUrl, apiKey, jsonHandler, clientAgents);
 client = new Client(config);
 }

 public void setHostUrl(String hostUrl) {
 this.hostUrl = hostUrl;
 }

 public void setApiKey(String apiKey) {
 this.apiKey = apiKey;
 }

 public void setJsonHandler(JsonHandler jsonHandler) {
 this.jsonHandler = jsonHandler;
 }

 public void setClientAgents(String[] clientAgents) {
 this.clientAgents = clientAgents;
 }
}
```

Create a Meilisearch client using the `afterPropertiesSet` method of `MeilisearchClientFactoryBean` using property values ​​parsed by BeanDefinitionParser.

## Conclusion

I defined the namespace directly and implemented the logic to register spring beans with `BeanDefinitionParser` and `FactoryBean`.
Through this process, I was able to understand how Spring works internally, and I look forward to what other experiences I will have in future feature implementations.
