---
title: "Spring Data Meilisearch #1 - 네임스페이스 기반 설정"
date: 2023-07-26T14:16:08+09:00
draft: false
series: 
  - Spring Data Meilisearch
categories: 
  - 프로젝트
tags:
  - ☕️ Java
  - 🍃 Spring
  - Meilisearch
  - 라이브러리
summary: >
  스프링 라이브러리에서 네임스페이스 기반 설정을 지원하는 방법
---

[Spring Data Meilisearch](https://github.com/junghoon-vans/spring-data-meilisearch) 프로젝트를 진행하면서 `Meilisearch 클라이언트`를 스프링 빈으로 등록하는 기능을 제공해야 했습니다.
스프링 빈에 등록하는 방법은 크게 두 가지가 있습니다.

- `XML 네임스페이스` 기반의 설정
- `자바 어노테이션` 기반의 설정

이번 포스팅에서는 스프링 라이브러리 단에서 `XML 기반의 설정`을 지원하는 방법에 대해 다뤄보겠습니다.

> 만약 `어노테이션 기반의 설정`을 지원하는 방법에 대해 알고 싶다면 [다음 글](/posts/project/spring-data-meilisearch/support-configuration-with-annotation)을 참고해주세요.

## 네임스페이스 설정하는 방법

`namespace.xml`에 다음과 같이 정의하면 Meilisearch 클라이언트를 생성하고 스프링 Bean으로 등록할 수 있습니다.

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

## 구현 방법

XML 기반 설정을 지원하기 위해서는 다음과 같은 구현이 필요합니다.

- 네임스페이스 기반 설정을 위해 `XSD 정의`
- `NamespaceHandler`를 통해 `네임스페이스 등록`
- `BeanDefinitionParser`를 통해 Bean 등록에 필요한 `속성 파싱`
- `FactoryBean`을 통해 Meilisearch `클라이언트 생성 및 Bean 등록`

### XSD 정의

XSD는 XML 문서의 구조와 내용을 정의하는 스키마 언어입니다. Spring에서는 XML을 통한 설정을 지원하기 위해 XSD를 사용합니다. Spring Data Meilisearch에서 XML 기반 설정을 지원하기 위해 `spring-meilisearch-1.0.xsd`라는 파일을 정의하였습니다.

> 해당 내용이 길어 접어두었으니 아래 버튼을 클릭하여 내용을 확인해주세요.

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

#### 엘리먼트 및 속성

위 XSD는 `meilisearch-client` 엘리먼트를 정의하고 있습니다. 해당 엘리먼트는 다음과 같은 속성을 가지고 있습니다.

- `host-url`
  - Meilisearch 서버의 주소를 지정합니다.
  - 기본값은 `http://localhost:7700`입니다.
- `api-key`
  - Meilisearch 서버의 API 키를 지정합니다.
- `json-handler`
  - JSON을 처리하는 라이브러리를 지정합니다.
  - 기본값은 `GSON`입니다.
- `client-agents`
  - Meilisearch 클라이언트의 에이전트를 지정합니다.
  - 기본값은 빈 배열입니다.

### NamespaceHandler

```java
public class MeilisearchNamespaceHandler extends NamespaceHandlerSupport {

  @Override
  public void init() {
    registerBeanDefinitionParser("meilisearch-client", new MeilisearchClientBeanDefinitionParser());
  }
}
```

앞선 XSD에서 정의한 `meilisearch-client` 엘리먼트를 `MeilisearchNamespaceHandler`를 통해 등록했습니다. 이때 함께 등록한 `MeilisearchClientBeanDefinitionParser`는 Bean 등록에 필요한 속성을 파싱하는 역할을 합니다. 뒤에서 자세히 다루겠습니다.

```properties
http\://www.vanslog.io/spring/data/meilisearch=io.vanslog.spring.data.meilisearch.config.MeilisearchNamespaceHandler
```

이후 `MeilisearchNamespaceHandler`를 `spring.handlers` 파일에 등록하여 Spring에서 해당 핸들러를 사용할 수 있도록 했습니다.

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

Meilisearch 클라이언트를 생성하는데 필요한 정보는 4가지입니다. 그래서 hostUrl, apiKey, clientAgents, jsonHandler를 파싱하여 `MeilisearchClientFactoryBean`이 Client 객체를 생성할 수 있도록 했습니다.

### FactoryBean

`MeilisearchClientFactoryBean`은 `FactoryBean`을 상속한 클래스로 Meilisearch 클라이언트를 생성하고 `Spring Bean`으로 등록하는 역할을 합니다.

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

BeanDefinitionParser가 파싱한 속성값들을 이용하여 `MeilisearchClientFactoryBean`의 `afterPropertiesSet` 메소드로 Meilisearch 클라이언트를 생성합니다.

## 마치며

네임스페이스를 직접 정의하고, `BeanDefinitionParser`와 `FactoryBean`으로 스프링 Bean을 등록하는 로직을 구현해봤습니다. 
이러한 과정에서 스프링의 내부 동작 방식에 대해 이해할 수 있었고, 앞으로 진행할 기능 구현에서 또 어떤 경험을 할 수 있을 지 기대됩니다.
