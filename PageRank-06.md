![截屏2023-02-25 下午5.33.15](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.33.15.png)

![截屏2023-02-25 下午5.35.43](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.35.43.png)

### 1.互联网的图表示

![截屏2023-02-25 下午5.41.43](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.41.43.png)

**pagerank是通过连接信息（别人引用自己）来计算图中节点重要度**。其中连接信息是引用这个节点的信息，**被重要网页引用的节点是重要的**。

### 2.理解PageRank的五个角度

#### 2.1迭代求解线性方程组($O(n^3)$，不推荐)

![截屏2023-02-25 下午5.50.59](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.50.59.png)

高斯消元法可以求解，但当节点数量较多的时候，计算相对复杂，可扩展性较差。![截屏2023-02-25 下午5.56.11](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.56.11.png)

在PageRank算法中，需要解决一个线性方程组的问题，其形式为 $Ax = b$，其中 $A$ 是一个 $n \times n$ 的矩阵，$x$ 是一个 $n$ 维向量，$b$ 是一个 $n$ 维向量。在实际求解中，通常使用**高斯消元法、LU分解**等算法来求解线性方程组，其时间复杂度为 $O(n^3)$，其中 $n$ 表示矩阵的维数。在PageRank算法中，$n$ 表示网页的数量，通常非常大，因此求解线性方程组的算法复杂度较高。

#### 2.2迭代左乘以M矩阵(推荐，幂迭代)

将之前的联立方程组求解转化为矩阵计算，具体如下：

第一列元素代表A只指向B和C，则第三、四行元素为$\frac{1}{2}$

![截屏2023-02-25 下午5.58.17](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.58.17.png)

![截屏2023-02-25 下午5.59.30](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午5.59.30.png)

![截屏2023-02-25 下午6.05.39](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.05.39.png)

#### 2.3矩阵的特征向量($O(n^3)$，不推荐)

![截屏2023-02-25 下午6.08.33](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.08.33.png)

![截屏2023-02-25 下午6.09.47](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.09.47.png)

![截屏2023-02-25 下午6.11.13](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.11.13.png)

#### 2.4随机游走(需模拟很多游走，不推荐)

![截屏2023-02-25 下午6.13.26](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.13.26.png)

![截屏2023-02-25 下午6.14.02](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.14.02.png)

#### 2.5马尔科夫链(和求解矩阵特征向量等价，不推荐)

求解PageRank其实就是在求解马尔科夫链的状态转移矩阵。

### 3.求解pagerank

上面五种方法中，幂迭代方法最好，**幂迭代将求解pagerank问题转化为矩阵乘法问题，而计算机最擅长做矩阵乘法问题。**下面是求解过程：

![截屏2023-02-25 下午6.22.17](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.22.17.png)

**求解流程：**

![截屏2023-02-25 下午6.22.43](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.22.43.png)

### 4.pagerank收敛分析

![截屏2023-02-25 下午6.25.56](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.25.56.png)

第一、二点同时满足才算是真正收敛。

![截屏2023-02-25 下午6.33.39](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.33.39.png)

对于非孤立、非周期性的马尔科夫链（互联网满足），可以满足第一点。

针对第二点，存在下面两个问题：（没有出链接、仅指向自己）这两种即便能计算出pagerank值，也不是我们想要的结果。

![截屏2023-02-25 下午6.36.42](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.36.42.png)

spider trap会使得所有节点都指向自己，使得节点陷入到某个节点中，求解出来的pagerank值没有意义。设置阻尼系数，使得某些节点会以一定概率随机访问其它节点。

Dead end节点的某些节点不会指向任何节点，会导致概率矩阵的某些列元素和为0，不满足概率矩阵的定义，无法得到最终pagerank值，将概率矩阵进行调整，使其满足概率矩阵要求。

![截屏2023-02-25 下午6.40.31](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.40.31.png)

**阻尼系数$\beta$**表示将一个网页跳转到原定网页的概率。**$1-\beta$的概率**，从一个网页**随机跳转**到其它网页，这样就能避免陷入到死胡同节点。（进入到某个节点就无法出来）

![截屏2023-02-25 下午6.41.53](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.41.53.png)

Dead-ends中数学上存在问题，这样会导致概率矩阵某些列元素的和为0，这种需要改写矩阵，使得概率矩阵每列元素的和都为1.

![截屏2023-02-25 下午6.42.15](/Users/jiamei5/Desktop/截屏2023-02-25 下午6.42.15.png)

![截屏2023-02-25 下午6.43.23](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午6.43.23.png)

阻尼系数=0.8，表示每5步进行一次随机传送。

**思考题：pagerank值可以自己刷高吗？pagerank为什么被称为“民主自治”算法？**

不能刷高，希望自己的pagerank值变高，只能让重要网页引用才能让自己的pagerank值变高。

### 5.pagerank升级变种

求解优化：通过mapredue进行分布式计算，提高运行效率。

应用：pagerank应用到推荐系统，寻找与指定节点最相似的节点（被同一个用户访问过的节点，更可能是相似的）

![截屏2023-02-25 下午7.04.26](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午7.04.26.png)

![截屏2023-02-25 下午7.06.18](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午7.06.18.png)

### 参考资料

![截屏2023-02-25 下午7.25.55](/Users/jiamei5/Library/Application Support/typora-user-images/截屏2023-02-25 下午7.25.55.png)

### 思考题

![截屏2023-02-25 下午7.24.28](/Users/jiamei5/Desktop/截屏2023-02-25 下午7.24.28.png)

1. ##### PageRank值可以被恶意刷高吗？

pagerank值不可以被恶意刷高，pagerank值主要取决于自己是否被重要节点引用来决定。

2. ##### PageRank为什么被称为"民主自治"算法？

pagerank是由其它节点“投票”来决定自己的重要度。

3. ##### 简述理解和计算PageRank的几个角度：线性方程组、矩阵左乘、特征向量、随机游走、马尔科夫链 

4. ##### PageEank假设某节点的所有出链接权重相同，这是否足够合理？如何改进？

在PageRank算法中，所有的出链接权重相同是一个合理的假设，因为它假设每个页面都是同等重要的，这是一个合理的起点。但是，在实际情况下，这个假设往往是不合理的。例如，某些页面可能比其他页面更受欢迎，或者某些页面可能被更多的其他网站链接，这些页面的链接应该被赋予更大的权重。

因此，为了改进PageRank算法，我们可以考虑对出链接权重进行调整。一种常见的做法是**通过对链接页面的重要性进行度量来给链接赋予不同的权重**。例如，我们可以将链接页面的PageRank值作为链接的权重，或者根据链接页面的流行程度或其他相关指标来计算链接的权重。这样可以更好地反映页面的真实权重和重要性，从而提高PageRank算法的准确性和有效性。

5. ##### Spider Trap和Dead End节点，会对求解PageRank值产生什么影响？如何改进？改进方法足够科学吗？

spider trap会使得所有节点都指向自己，使得所有节点最后都陷入到某个节点中，求解出来的pagerank值没有意义。设置阻尼系数，使得某些节点会以一定概率随机访问其它节点。

Dead end节点的某些节点不会指向任何节点，会导致概率矩阵的某些列元素和为0，不满足概率矩阵的定义，无法得到最终pagerank值，将概率矩阵进行调整，使其满足概率矩阵要求。

6. ##### Random Teleport概率B的值，会对求解PageRank值产生什么影响？

random teleport概率$\beta$值，会使得节点避免陷入到某个节点中，进而使得pagerank值有意义。

7. ##### PageRank、Personlized PageRank、 RandomWalk with Restart,这三种算法有什么区别？ 

PageRank是一种用于评估网页重要性的算法，它基于网页间的链接结构，通过迭代计算网页之间的关联性来确定网页的排名。

Personalized PageRank是一种基于PageRank算法的扩展，它允许用户对结果进行个性化定制。在Personalized PageRank中，除了考虑网页之间的链接关系，还会考虑用户对特定网页的兴趣程度，从而为用户提供更加个性化的结果。

Random Walk with Restart (RWR)也是一种基于图的算法，它通过模拟随机游走来确定节点的排名。与PageRank不同的是，RWR不仅考虑了节点之间的链接关系，还考虑了节点与图中其他节点的距离关系。在RWR中，节点的排名是由所有其他节点通过随机游走到该节点的概率之和计算得出的。

在总体上，PageRank和Personalized PageRank都是基于图的算法，通过考虑节点之间的链接关系来确定节点的排名。而RWR则更加注重节点之间的距离关系。此外，Personalized PageRank还允许用户对结果进行个性化定制，因此在实际应用中可能更加灵活。

8. ##### 求解PageRank时，为什么求解线性方程组和矩阵特征值的算法复杂度是$O(n^3)$?

求解PageRank时需要求解一个线性方程组或计算矩阵的特征值。其中，线性方程组的解法有很多种，例如高斯消元法、LU分解法、迭代法等等。而计算矩阵特征值的方法也有很多种，例如幂法、反幂法、QR分解法等等。在这些方法中，最常用的是迭代法和幂法。

迭代法求解线性方程组或计算矩阵特征值的算法复杂度通常是 $O(n^3)$。这是因为，在迭代法中，每次迭代都需要进行一次矩阵乘法，而矩阵乘法的时间复杂度为 $O(n^3)$。而且，在迭代法中，通常需要进行多次迭代，直到满足一定的精度要求为止。因此，总的算法复杂度就是 $O(n^3)$。

当然，也有其他方法可以用来求解线性方程组或计算矩阵特征值，例如快速傅里叶变换（FFT）、雅可比迭代法等等，这些方法的算法复杂度可能会更低。但是，迭代法和幂法是最常用的方法之一，也是最容易理解和实现的方法之一，因此在实际应用中仍然广泛使用。

9. ##### 求解PageRank时，为什么最终选择了简单粗暴的Power Iteration （幂迭代）？

幂迭代将求解pagerank问题转化为矩阵乘法问题，而计算机最擅长做矩阵乘法问题。

10. ##### Damping Factor设置为一个常数，是否足够科学？（看完抖音后更可能看微博，而不是公开课网站）

**"Damping Factor"（阻尼因子）是指在PageRank算法中用于调节链接传递权重的参数。当一个页面有多个链接指向其他页面时，阻尼因子可以决定权重被传递到其他页面的比例。**

将阻尼因子设置为一个常数可能并不足够科学，因为在不同的情况下，不同的阻尼因子可能会产生更好的结果。在实践中，通常需要对不同的网页或网络设置不同的阻尼因子，以便获得最佳的搜索结果。

此外，阻尼因子的选择也需要考虑到PageRank算法的一些局限性，例如算法可能会受到“链接农场”和“黑帽SEO”等技术的影响。因此，需要采用一些策略来解决这些问题，以提高PageRank算法的准确性和可靠性。

总之，将阻尼因子设置为一个常数可能会有一定的局限性，因此在实际应用中需要进行适当的调整和优化。