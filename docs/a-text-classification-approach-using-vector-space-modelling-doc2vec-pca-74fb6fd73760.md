# 一种使用向量空间模型(Doc2Vec)和 PCA 的文本分类方法

> 原文：<https://medium.com/swlh/a-text-classification-approach-using-vector-space-modelling-doc2vec-pca-74fb6fd73760>

假设给你很多带有正反标签的影评故事。现在，如果你必须开发一些自动算法来判断一篇新写的评论文章是正面的还是负面的，你如何用最简单的方法继续下去呢？

毫无疑问，你必须应用标准的 ML 分类技术(从问题陈述中，很容易理解这是一个分类问题)。但是这里的问题是大量的文本数据。这些不是数字，所以你怎么能让计算机/算法理解评论故事的重要性呢？让我们采取一步一步的方法

**从数据源获取数据**

在康乃尔大学的门户网站(【http://www.cs.cornell.edu/people/pabo/movie-review-data/】T2)可以找到电影评论故事的样本。可以下载“[极性数据集 v2.0](http://www.cs.cornell.edu/people/pabo/movie-review-data/review_polarity.tar.gz) ”。该数据集包含两个名为“pos”&“neg”的文件夹，每个文件夹下有 1000 个文件。每个文件都有以纯文本格式编写的评论文章。您可以使用下面的 Python 代码片段读取和打印数据集:

```
import pandas as pd
import pathlib as pldef _read_all_reviews_():
 all_reviews = [] for p in pl.Path('../data/txt_sentoken/pos').iterdir():
 file = open(p, 'r')
 all_reviews.append({'reviews_content': file.read(), 'category': 'positive'})
 file.close() for p in pl.Path('../data/txt_sentoken/neg').iterdir():
 file = open(p, 'r')
 all_reviews.append({'reviews_content': file.read(), 'category': 'negative'})
 file.close() all_reviews_df = pd.DataFrame(all_reviews)
 return all_reviews_dfprint(_read_all_reviews_())
```

**ML 管道的概念**

任何 ML 算法执行的最佳实践都包括管道创建。然而，在不创建管道的情况下，您也可以执行 ML 算法。但是流水线让你的生活变得简单。我们先来了解一下‘管道’是什么意思？

大多数 ML 算法需要对票价数据进行预处理。所有这些预处理和实际算法都可以被配置为单独的可重用步骤。所有这些步骤连接在一个具有入口和出口的单一实体中，称为“管道”。现在让我们看看在我们的问题中有哪些必要的步骤。

**创建矢量空间模型**

管道中的第一步是将数据转换成数值，因为它目前是纯文本格式。有标准型号，如“Tf-Idf”、“Word2Vec”、“Doc2Vec”等。所有这些都被称为文本分析中的“向量空间模型”,其中每一个都只给出文本数据的数字向量表示。该向量作为文档的特征向量工作，并确定有多少个特征。我使用“Doc2Vec”作为这个问题的模型，因为每个评论都可以被视为一个单独的文档，并且“Doc2Vec”在理解文本的上下文含义方面非常有效(与 Tf-Idf 相比，Tf-Idf 只是一个纯粹的基于频率的模型)。文档的“Doc2Vec”是通过对照随机采样的单词的邻近单词训练神经网络而生成的，反之亦然。这里，只有学习到的隐藏层的权重才是关注的问题，并被取为“Doc2Vec”值。事实上有两种技术:分布式内存(DM)和分布式单词包(DBOW)。有关更多详情，您可以查看以下资源:

*   [稀有科技的 Doc2Vec 教程](https://rare-technologies.com/doc2vec-tutorial/)
*   [分布式陈述语句&文档](https://cs.stanford.edu/~quocle/paragraph_vector.pdf)

以下 Python 代码片段用作为文档创建 Doc2Vec 的管道步骤:

```
from sklearn.pipeline import Pipeline
from sklearn.decomposition import PCA
from gensim.models.doc2vec import TaggedDocument, Doc2Vec
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from gensim.parsing.preprocessing import preprocess_string
from sklearn import utils
from tqdm import tqdmclass Doc2VecTransformer(BaseEstimator): def __init__(self, vector_size=100, learning_rate=0.02, epochs=20):
 self.learning_rate = learning_rate
 self.epochs = epochs
 self._model = None
 self.vector_size = vector_size
 self.workers = multiprocessing.cpu_count() def fit(self, df_x, df_y=None):
 tagged_x = [TaggedDocument(preprocess_string(row['reviews_content']), [index]) for index, row in df_x.iterrows()]
 model = Doc2Vec(documents=tagged_x, vector_size=self.vector_size, workers=self.workers) for epoch in range(self.epochs):
 model.train(utils.shuffle([x for x in tqdm(tagged_x)]), total_examples=len(tagged_x), epochs=1)
 model.alpha -= self.learning_rate
 model.min_alpha = model.alpha self._model = model
 return self def transform(self, df_x):
return np.asmatrix(np.array([self._model.infer_vector(preprocess_string(row['reviews_content'])) for index, row in df_x.iterrows()]))
```

在上面的代码片段中，“vector_size”参数决定了每个文档的向量长度。基本上，它是如上所述的文档的提取特征的数量。这些特征是“虚拟的性质”,在物理上很难想象。你可以说，在物理上它没有任何重要性，但是这些为数学建模增加了巨大的价值。您可以看到，该模型以调整后的学习速率反复训练了多次。只是为了更好的结果。“vector_size”和“learning_rate”可以作为超参数来调整模型。稍后我会解释。最终结果是，Doc2Vec 特性作为矩阵返回，其中行是文档编号，列是特性值。

**doc 2 vec 的 PCA 建模**

正如您所看到的，Doc2Vec 可以生成大量的特性，这取决于您在参数“vector_size”中传递的值，因此将它简化为相关的特性集总是很方便的。主成分在这里可以起到很好的作用。因此，您的下一步工作将是对 Doc2Vec 进行 PCA 转换。主要组件也是虚拟特征，如“Doc2Vec ”,难以可视化/概念化，但有利于数学建模。您可以在 PCA transformer 中将“n_components”作为参数(主成分的数量)传递。结果你会得到主成分分析向量。显然，你的“n_components”应该比“vector_size”小得多，可能是它的二分之一或三分之一。

**拟合逻辑回归分类器**

完成 PCA 后，您可以将 PCA 向量放入二元逻辑回归分类器中(因为这里的输出是分类变量，只能有两个值:“正”和“负”)，这将是您管道中的最后一步。根据 PCA 向量值，该分类器将预测电影评论故事是正面的还是负面的。

下面的 python 代码片段显示了整个管道:

```
def train_and_build_model():
 all_reviews_df = _read_all_reviews_()
 train_x_df, test_x_df, train_y_df, test_y_df = train_test_split(all_reviews_df[['reviews_content']], all_reviews_df[['category']]) pl = Pipeline(steps=[('doc2vec', Doc2VecTransformer(vector_size=220)),
 ('pca', PCA(n_components=100)),
 ('logistic', LogisticRegression())
 ])
 pl.fit(train_x_df[['reviews_content']], train_y_df[['category']])
 predictions_y = pl.predict(test_x_df[['reviews_content']])
 print('Accuracy: ', metrics.accuracy_score(y_true=test_y_df[['category']], y_pred=predictions_y))
```

现在，你可能会想我是从哪里得到“n_components”的值 100 和“vector_size”的值 220 的。你可以在这里传递任何值。一般对于 Doc2Vec，“vector_size”保持在 100 & 300 之间。但是对于主成分分析，这取决于你想要保留多少主成分来保持数据中足够的可变性。“向量大小”和“n 个组件”都是这里的“超参数”。

**用网格搜索调整超参数**

Python 中的网格搜索库为您提供了找到最佳超参数集的机制。您可以提供一系列可能的值，以便从分类器模型中获得最佳准确性。以下 Python 代码片段可以为您提供这一点:

```
from sklearn.model_selection import GridSearchCVdef train_long_range_grid_search():
 all_reviews_df = _read_all_reviews_()
 train_x_df, test_x_df, train_y_df, test_y_df = train_test_split(all_reviews_df[['reviews_content']], all_reviews_df[['category']]) pl = Pipeline(steps=[('doc2vec', Doc2VecTransformer()),
 ('pca', PCA()),
 ('logistic', LogisticRegression())
 ]) param_grid = {
 'doc2vec__vector_size': [x for x in range(100, 250)],
 'pca__n_components': [x for x in range(1, 50)]
 }
 gs_cv = GridSearchCV(estimator=pl, param_grid=param_grid, cv=5, n_jobs=-1,
 scoring="accuracy")
 gs_cv.fit(train_x_df[['reviews_content']], train_y_df[['category']]) print("Best parameter (CV score=%0.3f):" % gs_cv.best_score_)
 print(gs_cv.best_params_)
 predictions_y = gs_cv.predict(test_x_df[['reviews_content']])
 print('Accuracy: ', metrics.accuracy_score(y_true=test_y_df[['category']], y_pred=predictions_y))
```

但上面的一个将花费大量的时间来获得最佳设置。因此，简而言之，你可以减少可能的范围，仍然得到或多或少的最优设置。下面的代码片段就是为此准备的:

```
def train_short_range_grid_search():
 all_reviews_df = _read_all_reviews_()
 train_x_df, test_x_df, train_y_df, test_y_df = train_test_split(all_reviews_df[['reviews_content']], all_reviews_df[['category']]) pl = Pipeline(steps=[('doc2vec', Doc2VecTransformer()),
 ('pca', PCA()),
 ('logistic', LogisticRegression())
 ]) param_grid = {
 'doc2vec__vector_size': [200, 220, 250],
 'pca__n_components': [50, 75, 100]
 }
 gs_cv = GridSearchCV(estimator=pl, param_grid=param_grid, cv=3, n_jobs=-1,
 scoring="accuracy")
 gs_cv.fit(train_x_df[['reviews_content']], train_y_df[['category']]) print("Best parameter (CV score=%0.3f):" % gs_cv.best_score_)
 print(gs_cv.best_params_)
 predictions_y = gs_cv.predict(test_x_df[['reviews_content']])
 print('Accuracy: ', metrics.accuracy_score(y_true=test_y_df[['category']], y_pred=predictions_y))
```

从上面我们得到的准确度分数是 55%左右。这可以通过选择正确的模型或方法来改善。你可以在 [github 库](https://github.com/avisheknag17/public_ml_models)中获得完整的代码库。

**结论&方法优化**

在机器学习中，有一点是非常明显的“没有模型是绝对正确或错误的”。每种模式都有其利弊。上述问题也可以用不同的方式解决。事实上，如果我采取完整的深度学习方法，那么它可以给出更好的结果。但是我的意图是提供一个使用 PCA 和逻辑回归的技术的演示，这样你就可以把它应用于任何文本分析问题。

我的方法是“如何在‘线性分类模型’中使用‘非线性特征’”。“Doc2Vec”肯定是使用神经网络从文档中提取的非线性特征，而逻辑回归是线性和参数分类模型。在机器学习中解决问题的四种组合是很常见的:“简单特征-简单模型”、“简单特征-复杂模型”、“复杂特征-简单模型”和“复杂特征-复杂模型”。我的方法属于“复杂特性-简单模型”类别。对数学概念和软件工程经验和技能的扎实掌握将赋予你选择正确方法的直觉/分析能力。

不管怎样！！下次会提出另一个话题。在那之前，享受“机器学习”和“快乐编码”

参考资料:

a) [Python-Gensim 教程](https://radimrehurek.com/gensim/tutorial.html)

b) [带数学概念的 PCA 教程](http://www.cs.otago.ac.nz/cosc453/student_tutorials/principal_components.pdf)

c) [带数学概念的逻辑回归教程](http://ufldl.stanford.edu/tutorial/supervised/LogisticRegression/)

最近，我写了一本关于 ML([https://twitter.com/bpbonline/status/1256146448346988546](https://twitter.com/bpbonline/status/1256146448346988546))的书

*原载于 2019 年 3 月 21 日*[*medium.com*](/@avisheknag17/a-text-classification-approach-using-vector-space-modelling-pca-2a23fc2d66d7)*。*

[![](img/308a8d84fb9b2fab43d66c117fcc4bb4.png)](https://medium.com/swlh)

## 这篇文章发表在 [The Startup](https://medium.com/swlh) 上，这是 Medium 最大的创业刊物，拥有+436，678 名读者。

## 在这里订阅接收[我们的头条新闻](https://growthsupply.com/the-startup-newsletter/)。

[![](img/b0164736ea17a63403e660de5dedf91a.png)](https://medium.com/swlh)