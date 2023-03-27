# 支持弹性搜索中的完全匹配

> 原文：<https://medium.com/swlh/favouring-full-matches-in-elasticsearch-f3169982091d>

## 如何将最相关的匹配放在最上面，同时保留不太相关但仍然相关的结果

![](img/beed899057b1fb09e24c36224793f85e.png)

Photo by [Nick Karvounis](https://unsplash.com/@nickkarvounis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 问题是

为了说明这个问题，假设我们有两个弹性搜索索引:一个包含一些画家的列表，另一个包含作曲家的列表。

要亲自尝试这些示例，您可以在 Elasticsearch 实例上运行以下批量索引请求:

```
POST /_bulk
{"index": {"_index": "painters"}}
{"name": "**Vincent van Gogh**"}
{"index": {"_index": "painters"}}
{"name": "**Rembrandt van Rijn**"}
{"index": {"_index": "painters"}}
{"name": "**Frans Hals**"}
{"index": {"_index": "painters"}}
{"name": "**Johann Adam Ackermann**"}
{"index": {"_index": "painters"}}
{"name": "**Piet Mondriaan**"}
{"index": {"_index": "painters"}}
{"name": "**Claude Monet**"}
{"index": {"_index": "painters"}}
{"name": "**Jackson Pollock**"}
{"index": {"_index": "painters"}}
{"name": "**Andy Warhol**"}
{"index": {"_index": "painters"}}
{"name": "**Frida Kahlo**"}
{"index": {"_index": "painters"}}
{"name": "**Johannes Vermeer**"}
{"index": {"_index": "painters"}}
{"name": "**Leonardo da Vinci**"}
{"index": {"_index": "painters"}}
{"name": "**Pieter Breugel**"}
{"index": {"_index": "composers"}}
{"name": "**Johann Sebastian Bach**"}
{"index": {"_index": "composers"}}
{"name": "**Johann Christoph Bach**"}
{"index": {"_index": "composers"}}
{"name": "**Johann Ambrosius Bach**"}
{"index": {"_index": "composers"}}
{"name": "**Clara Schumann**"}
```

现在，为了说明这个问题，让我们搜索名字为“约翰·塞巴斯蒂安·巴赫”的人:

```
GET /painters,composers/_search
{
  "query": {
    "match": {
      "name": "johann sebastian bach"
    }
  }
}
```

它返回:

```
{
  ...
  "hits" : {
    ...
    "hits" : [
      {
        "_index" : "painters",
        "_type" : "_doc",
        "_id" : "xIxFCWsB7VA-_8kTLQH0",
        "_score" : 1.9334917,
        "_source" : {
          "name" : "**Johann Adam Ackermann**"
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "zYxFCWsB7VA-_8kTLQH1",
        "_score" : 1.8485742,
        "_source" : {
          "name" : "**Johann Sebastian Bach**"
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "zoxFCWsB7VA-_8kTLQH1",
        "_score" : 0.6877716,
        "_source" : {
          "name" : "Johann Christoph Bach"
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "z4xFCWsB7VA-_8kTLQH1",
        "_score" : 0.6877716,
        "_source" : {
          "name" : "Johann Ambrosius Bach"
        }
      }
    ]
  }
}
```

哇，这是怎么回事？与我们的预期相反，巴赫并不是最靠前的搜索结果。虽然我们在`composers`索引中找到了“约翰·塞巴斯蒂安·巴赫”(分数约为 1.85)的字面匹配，但画家“约翰·亚当·阿克曼”(Johann Adam Ackermann)的分数更高(约为 1.93)，他只匹配了我们搜索查询的一小部分！

# 请解释一下

和往常一样，我们可以[向 Elasticsearch 寻求解释](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-explain.html):

```
GET /painters,composers/_search**?explain=true**
{
  "query": {
    "match": {
      "name": "johann sebastian bach"
    }
  }
}
```

这给了我们一个提示:与我们人类不同，Elasticsearch 不知道“约翰·塞巴斯蒂安·巴赫”这个名字是一个连贯的单位，所以它单独搜索每个术语。

1.  首先，一个[标记器](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenizers.html)将查询分成三个词:`johann OR sebastian OR bach`。
2.  然后，Elasticsearch 分别搜索每个术语。
3.  最后，它通过合并每个术语的分数来计算总分。

所以 Elasticsearch 默认运行[一个](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html#query-dsl-match-query-boolean) `[OR](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html#query-dsl-match-query-boolean)` [查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html#query-dsl-match-query-boolean)。也就是说，*至少有一个*的搜索词必须匹配，但不一定都要匹配。这解释了为什么 Johann Adam Ackermann 包含在结果中，尽管只有一个词(“Johann”)与我们的查询匹配。

## 逆文档频率咬我们

但是这还没有回答为什么阿克曼的排名比巴赫高。是什么让阿克曼更相关？这与【Elasticsearch 如何计算相关性有关:它依赖于 [TF/IDF](https://www.elastic.co/guide/en/elasticsearch/guide/current/scoring-theory.html#tfidf) 算法。困扰我们的是 IDF(倒排文档频率)部分:对于一个给定的搜索词，它出现在越多的文档中，它被认为越不相关。因此，在许多文档中出现的术语权重较低。一般来说，这是有意义的:如果你搜索**'**the well-tempered clavier '，你不会对所有包含诸如“the”等常见术语的文档感兴趣，而只会对少数提到键盘的文档感兴趣，最好是 the-tempered 那种。

如果你看上面的数据，你会发现这两个指数加起来有四个约翰和三个巴赫。所以这仍然使巴赫成为更独特的相关术语，不是吗？[可惜不是，因为](https://www.elastic.co/guide/en/elasticsearch/guide/current/scoring-theory.html#_putting_it_together):

> 每个字段都有自己的倒排索引，因此，为了 TF/IDF 的目的，字段的值就是文档的值。

也就是说，我们需要在字段级别区分*，而不是(仅)在索引级别区分*。(尽管两个索引中的字段都称为`name`，但它们属于两个不同索引的事实使它们成为两个字段。)在这种情况下，我们在`painters.name`领域只有一个约翰(阿克曼)，在`composers.name`领域有三个，这确实将画家的相关性提升到了作曲家(约翰·塞巴斯蒂安·巴赫)之上。

# 解决方案 1:仅匹配全部结果

正如我们上面看到的，默认情况下，Elasticsearch 使用`OR`来组合这些术语。那么，一个显而易见的解决方案就是告诉 Elasticsearch 匹配所有的搜索词*。您可以通过将操作员更改为`AND`来完成此操作:*

```
GET /painters,composers/_search
{
  "query": {
    "match": {
      "name": {
        "query": "johann sebastian bach",
 **"operator": "and"**
      }
    }
  }
}
```

瞧，我们只有一个结果，那就是 JSB。完成了吗？

那么，如果用户将巴赫与另一位著名作曲家的[混淆，而搜索`johann van bach`呢？那个查询现在返回零个结果(因为`van`在 Bach 的名字中找不到)，这对我们的用户来说有点太苛刻了。](https://en.wikipedia.org/wiki/Ludwig_van_Beethoven)

# 解决方案 2:支持完整的结果

我们可以通过用`[minimum_should_match](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-minimum-should-match.html)`替换自定义的`operator`来解决这个问题:

```
GET /painters,composers/_search
{
  "query": {
    "match": {
      "name": {
        "query": "johann sebastian bach",
 **"minimum_should_match": "2<75%"**
      }
    }
  }
}
```

`2<75%`意思是:

*   如果你只提供两个搜索词(如`johann bach`，它们必须都匹配(`johann AND bach`)；
*   但是如果您提供两个以上的术语(例如`johann van bach`，只有 75%(向下取整)必须匹配，所以这归结为`(johann AND van) OR (van AND bach) OR (johann AND bach)`。

现在我们的搜索结果只包含三个 Bachs，Johann Sebastian 排名最高:

```
{
  ...
  "hits" : {
    ...
    "hits" : [
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "zYxFCWsB7VA-_8kTLQH1",
        "_score" : 1.8485742,
        "_source" : {
          "name" : "**Johann Sebastian Bach**"
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "zoxFCWsB7VA-_8kTLQH1",
        "_score" : 0.6877716,
        "_source" : {
          "name" : "Johann Christoph Bach"
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "z4xFCWsB7VA-_8kTLQH1",
        "_score" : 0.6877716,
        "_source" : {
          "name" : "Johann Ambrosius Bach"
        }
      }
    ]
  }
}
```

然而，这从我们的搜索结果中删除了画家 Johann Ackermann，我们可能希望他作为‘Johann’的部分匹配返回。

![](img/e628feb07d2a5417ecc221bc966763dc.png)

By [Ian McFegan](https://www.flickr.com/photos/ian_wilson_49/35145266852/in/photolist-VxEFoL-5bSHpW-VGb4kJ-kvEQ7y-7mDxN9-UYSnJj-m92wm-bsmDsx-kvCJVg-5R2fQC-mcaVGk-9Zo6Ap-bsmEg8-2dZC2sM-VV8yPc-bkUvDx-4MMPJQ-djnkYg-b7q7AD-VVaLtv-mgzVct-djnmpH-Tht4sb-dCmDkL-2cs5Zgi-TNkmW5-WpHYmR-UWjRdK-51xvba-4LXex2-9eiCvo-227UwBp-WHYzCw-cqvYWo-7Y3q9a-7DZi34-21SVdxg-M6tx8p-eEEW5d-TRVPhU-qCWqbB-r8FxW5-dYSjTP-DPdrVx-nFVnUQ-beX6Xc-9r3Kvp-W1VBjj-7caBiw-5bFTjg)

# 解决方案 3:以正确的方式支持完整的结果

所以把操作符改成`AND`(方案 1)和提供一个`minimum_should_match`(方案 2)都*太严格*。一个更好、更灵活的解决方案是支持全部结果*，但仍然包含部分匹配的长尾。*

解决这个问题的方法是利用复合布尔查询中`should`子句的工作方式。`should`关键字类似于常规的`OR`,但在一个重要方面有所不同:匹配的子句越多，文档就被认为越相关。

这使得将我们喜欢的结果指定为一组`should`子句变得非常自然。我们首先将原始查询包装在一个 bool 查询中:

```
GET /painters,composers/_search
{
  "query": {
 "bool": {
      "should": [        **{
          "match": {
            "name": "johann sebastian bach"
          }
        }**
]
    }  }
}
```

虽然这还没有改变我们的搜索结果，但它为添加更多的`should`子句开辟了道路。因此，在此基础上，如果一个文档包含与查询顺序相同的术语组合，我们可以认为它更相关。用 Elasticsearch 的说法，这是一个 [*短语匹配*](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html) *。为此简单地增加一个`should`条款:*

```
GET /painters,composers/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "johann sebastian bach"
          }
        },
 **{
          "match_phrase": {
            "name": "johann sebastian bach"
          }
        }**      ]
    }
  }
}
```

最后，我们可以将`minimum_should_match`查询添加回我们的复合查询:

```
GET /painters,composers/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "johann sebastian bach"
          }
        },
        {
          "match_phrase": {
            "name": "johann sebastian bach"
          }
        },
        **{
          "match": {
            "name": {
              "query": "johann sebastian bach",
              "minimum_should_match": "2<75%"
            }
          }
        }**
      ]
    }
  }
}
```

这给了我们想要的:

```
{
  ...
  "hits" : {
    ...
    "hits" : [
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "j2VWR2sBKYAfkVJkD-Xu",
        "_score" : **5.5457225**,
        "_source" : {
          "name" : **"Johann Sebastian Bach"**
        }
      },
      {
        "_index" : "painters",
        "_type" : "_doc",
        "_id" : "hmVWR2sBKYAfkVJkD-Xu",
        "_score" : **1.9334917**,
        "_source" : {
          "name" : **"Johann Adam Ackermann"**
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "kGVWR2sBKYAfkVJkD-Xu",
        "_score" : 1.3755432,
        "_source" : {
          "name" : "Johann Christoph Bach"
        }
      },
      {
        "_index" : "composers",
        "_type" : "_doc",
        "_id" : "kWVWR2sBKYAfkVJkD-Xu",
        "_score" : 1.3755432,
        "_source" : {
          "name" : "Johann Ambrosius Bach"
        }
      }
    ]
  }
}
```

# 结论

所有的搜索都是在精确度和召回率之间的权衡。为了在这两者之间取得良好的平衡，你可以使用`should`从句更加具体地描述你想要的结果。这首先给你最好的结果，同时保留较少但仍然相关的结果。