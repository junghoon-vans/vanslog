---
title: "Introduction"
date: 2023-06-28T13:48:31+09:00
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
  Introducing the reasons and goals for starting the Spring Data Meilisearch project.
---

## Open source search engine trends

The reason I became interested in [Meilisearch](https://www.meilisearch.com) was because I discovered meaningful data while looking at trends in open source search engines. The graph below is data extracted from [OSS Insight](https://ossinsight.io), showing the number of stars of search engine projects registered on GitHub from 2011 to 2021.

![Search engine trends](https://vanslog.s3.ap-northeast-2.amazonaws.com/image/project/Search+Engine+-+Stars.png)

[Elasticsearch](https://www.elastic.co/kr/elasticsearch/) continues to remain in first place as the most used search engine. What's really interesting is that Meilisearch has shown steady growth since its launch in 2018 and is currently in second place.

### Meilisearch vs Elasticsearch

Elasticsearch specializes in `large-scale data processing` and has strengths in data analysis, including data visualization through integration with Kibana.
Meilisearch, on the other hand, focuses on `End User Experience`. It provides a powerful typo detection function, intuitive user experience, and fast search speed.

| | Meilisearch | Elasticsearch |
| ------------ |----------------------------------- | ---------------------- |
| Source code | licensing MIT (Fully open-source) | SSPL (Not open-source) |
| Built with | Rust | Java |
| Data storage | Disk with Memory Mapping | Disk with RAM cache |

I briefly summarized [Comparison with Elasticsearch](https://www.meilisearch.com/docs/learn/what_is_meilisearch/comparison_to_alternatives) in a table on the Meilisearch official site. Meilisearch is written in Rust to provide fast response, and uses disk and memory mapping for data storage. Although Meilisearch is not always better than Elasticsearch, it is suitable for site or in-app search applications such as e-commerce, document, and content search.

## Difficulties with Spring integration

As Meilisearch is a search engine targeting end users, it provides client libraries in various languages. However, I realized that it doesn't provide a way to integrate directly with Spring. Therefore, there is currently the hassle of having to write queries directly using a Java client.

### Spring Data library

Spring provides interfaces for easy integration with various data stores through a project called [Spring Data](https://spring.io/projects/spring-data). Search engines such as Elasticsearch and Solr all come with Spring Data libraries.

To enable Meilisearch to be conveniently used in Spring, we thought the best way would be to provide an implementation of Spring Data. So we started the [Spring Data Meilisearch](https://github.com/junghoon-vans/spring-data-meilisearch) project.

### Goals of Spring Data Meilisearch

The goal is to make it easy to integrate and use Meilisearch into Spring projects. For this you will need the following features:

- Repository interface provided
- Provides methods corresponding to various queries
- Integration with Meilisearch client
- Provides settings through Spring namespace
- Provide settings through annotation
- Settings provided through CDI
- And so on...

### Conclusion

The project has just begun and there is a lot of work to do. But I think it's always fun to develop useful solutions yourself. I've always wanted to create my own Java library, and I think this will be a good opportunity to understand the internal structure of the Spring Data library.

## References

- [OSS Insight - Search Engine](https://ossinsight.io/collections/search-engine/)
- [Meilisearch - comparison to alternatives](https://www.meilisearch.com/docs/learn/what_is_meilisearch/comparison_to_alternatives)
