---
title: "Spring Data MeiliSearch #0 - 프로젝트 소개"
date: 2023-06-28T13:48:31+09:00
draft: false
series: 
  - Spring Data MeiliSearch
categories:
  - 프로젝트
tags:
  - ☕️ Java
  - 🍃 Spring
  - Meilisearch
  - 라이브러리
summary: >
  Spring Data MeiliSearch 프로젝트를 시작하게 된 계기와 목표에 대해 소개합니다.
---

# 오픈소스 검색엔진 동향

[Meilisearch](https://www.meilisearch.com)에 관심을 가지게 된 계기는 오픈소스 검색엔진의 동향을 살펴보는 과정에서 유의미한 데이터를 발견했기 때문입니다. 아래 그래프는 [OSS Insight](https://ossinsight.io)에서 발췌한 데이터로, 2011년부터 2021년까지 Github에 등록된 검색 엔진 프로젝트의 스타 수를 나타낸 것입니다. 

![검색엔진 동향](https://vanslog.s3.ap-northeast-2.amazonaws.com/image/project/Search+Engine+-+Stars.png)

[Elasticsearch](https://www.elastic.co/kr/elasticsearch/)는 가장 많이 사용하는 검색엔진답게 지속적으로 1위를 유지하고 있습니다. 정말 흥미로운 점은 Meilisearch가 2018년에 등장한 이후 꾸준한 성장세를 보여주었으며, 현재 2위를 차지하고 있다는 점입니다.

## Meilisearch vs Elasticsearch

Elasticsearch는 `대규모 데이터 처리`에 특화되어 있으며, Kibana와 통합하여 데이터 시각화를 하는 등 데이터 분석에 강점을 가지고 있습니다.
반면, Meilisearch는 `최종 사용자 경험`에 초점을 맞추고 있습니다. 강력한 오타 검색 기능과, 직관적인 사용자 경험 그리고 빠른 검색 속도를 제공합니다.

|              | Meilisearch	                      | Elasticsearch          |
| ------------ |----------------------------------- | ---------------------- |
| Source code  | licensing	MIT (Fully open-source)	| SSPL (Not open-source) |
| Built with	 | Rust                               | Java                   |
| Data storage | Disk with Memory Mapping      	    | Disk with RAM cache    |

Meilisearch 공식 사이트에서 [Elasticsearch와 비교한 내용](https://www.meilisearch.com/docs/learn/what_is_meilisearch/comparison_to_alternatives)을 표로 간단히 정리해보았습니다. Meilisearch는 빠른 응답을 제공하기 위해 Rust로 작성되었으며, 데이터 스토리지로 디스크와 메모리 매핑을 사용합니다. 이러한 특징으로 인해 Elasticsearch보다 빠른 검색 속도를 제공합니다.

Meilisearch가 항상 Elasticsearch보다 항상 우수한 것은 아니지만, 전자 상거래, 문서 및 콘텐츠 검색과 같은 사이트나 인앱 검색에 적용하기 적합합니다.

# Spring 통합의 어려움

Meilisearch는 최종 사용자를 겨냥한 검색엔진인 만큼, 다양한 언어의 클라이언트 라이브러리를 제공하고 있습니다. 하지만 Spring과 직접 통합하는 방법은 제공하고 있지 않다는 사실을 깨달았습니다. 그렇기 때문에 현재는 자바 클라이언트를 사용하여 직접 쿼리를 작성해야 하는 번거로움이 있습니다.

## Spring Data 라이브러리

Spring은 [Spring Data](https://spring.io/projects/spring-data)라는 프로젝트를 통해 다양한 데이터 스토리지와 쉽게 통합할 수 있는 인터페이스를 제공합니다. Elasticsearch나 Solr와 같은 검색엔진은 모두 Spring Data 라이브러리가 제공되고 있습니다.

Meilisearch를 Spring에서 편리하게 사용할 수 있도록 하기 위해 Spring Data의 구현체를 제공하는 것이 가장 좋은 방법이라고 생각했습니다. 그래서 시작한 것이 [Spring Data Meilisearch](https://github.com/junghoon-vans/spring-data-meilisearch) 프로젝트입니다.

## Spring Data Meilisearch의 목표

Meilisearch를 Spring 프로젝트에 쉽게 통합하여 사용할 수 있도록 하는 것이 목적입니다. 이를 위해서는 다음과 같은 기능이 필요합니다.

- Repository 인터페이스 제공
- 다양한 쿼리에 대응되는 메소드 제공
- Meilisearch 클라이언트와 연동
- Spring 네임스페이스를 통한 설정 제공
- 애노테이션을 통한 설정 제공
- CDI를 통한 설정 제공
- 등등..

## 마치며

프로젝트는 이제 막 시작되었고 할 일 투성이입니다. 하지만 유용한 솔루션을 직접 개발하는 것은 늘 즐거운 일이라고 생각합니다. 평소 자바 라이브러리를 직접 만들어보고 싶었는데, 이번 기회가 Spring Data 라이브러리의 내부 구조를 이해하는 좋은 계기가 될 것 같습니다.

# 참고문헌

- [OSS Insight - Search Engine](https://ossinsight.io/collections/search-engine/)
- [Meilisearch - comparison to alternatives](https://www.meilisearch.com/docs/learn/what_is_meilisearch/comparison_to_alternatives)
