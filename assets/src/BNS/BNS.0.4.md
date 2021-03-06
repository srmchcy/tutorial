### Deeplearning Algorithms tutorial
谷歌的人工智能位于全球前列，在图像识别、语音识别、无人驾驶等技术上都已经落地。而百度实质意义上扛起了国内的人工智能的大旗，覆盖无人驾驶、智能助手、图像识别等许多层面。苹果业已开始全面拥抱机器学习，新产品进军家庭智能音箱并打造工作站级别Mac。另外，腾讯的深度学习平台Mariana已支持了微信语音识别的语音输入法、语音开放平台、长按语音消息转文本等产品，在微信图像识别中开始应用。全球前十大科技公司全部发力人工智能理论研究和应用的实现，虽然入门艰难，但是一旦入门，高手也就在你的不远处！
AI的开发离不开算法那我们就接下来开始学习算法吧！

#### 多项式朴素贝叶斯(Multinomial Naive Bayes)

 多项式朴素贝叶斯(Multinomial Naive Bayes)算法，用于文本分类(这个领域中数据往往以词向量表示，尽管在实践中 tf-idf向量在预测时表现良好)的两大经典朴素贝叶斯算法之一。分布参数由每类<img width="10" align="center" src="../../images/206.jpg" />的<img width="160" align="center" src="../../images/207.jpg" />向量决定， 式中 n 是特征的数量(对于文本分类，是词汇量的大小)<img width="30" align="center" src="../../images/208.jpg" />是样本中属于类 y 中特征i概率<img width="80" align="center" src="../../images/209.jpg" /> 。
 
参数 <img width="30" align="center" src="../../images/210.jpg" />使用平滑过的最大似然估计法来估计，即相对频率计数:

<p align="center">
<img width="150" align="center" src="../../images/211.jpg" />
</p>

式中<img width="160" align="center" src="../../images/212.jpg" />是训练集T中特征 i 在类 y 中出现的次数，<img width="160" align="center" src="../../images/213.jpg" />是类y中出现所有特征的计数总和。

先验平滑因子<img width="50" align="center" src="../../images/214.jpg" />应用于在学习样本中没有出现的特征，以防在将来的计算中出现0概率输出。 把  <img width="50" align="center" src="../../images/215.jpg" /> 被称为拉普拉斯平滑(Lapalce smoothing)，而<img width="50" align="center" src="../../images/216.jpg" />被称为利德斯通(Lidstone smoothing)。



#### 应用实例
```python
#GaussianNB differ from MultinomialNB in these two method:
# _calculate_feature_prob, _get_xj_prob
class GaussianNB(MultinomialNB):
        """
        GaussianNB inherit from MultinomialNB,so it has self.alpha
        and self.fit() use alpha to calculate class_prior
        However,GaussianNB should calculate class_prior without alpha.
        Anyway,it make no big different

        """
        #calculate mean(mu) and standard deviation(sigma) of the given feature
        def _calculate_feature_prob(self,feature):
                mu = np.mean(feature)
                sigma = np.std(feature)
                return (mu,sigma)

        #the probability density for the Gaussian distribution 
        def _prob_gaussian(self,mu,sigma,x):
                return ( 1.0/(sigma * np.sqrt(2 * np.pi)) *
                        np.exp( - (x - mu)**2 / (2 * sigma**2)) )

        #given mu and sigma , return Gaussian distribution probability for target_value
        def _get_xj_prob(self,mu_sigma,target_value):
                return self._prob_gaussian(mu_sigma[0],mu_sigma[1],target_value)

```
