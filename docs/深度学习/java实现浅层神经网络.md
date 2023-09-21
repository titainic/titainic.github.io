---
layout: default
title: java实现浅层神经网络
parent: 深度学习
nav_order: 1
---

{: .highlight }
根据吴恩达深度学习视频，结合自己理解，使用java代码基于DL4J框架实现浅层神经网络。

{: .note-title }
> 目的
>
> 更加深刻理解神经网络的反向传播思想

---

## 准备数据



<div class="code-example" markdown="1">
数据是400行，3列，前两列为特征值，最后一列为标签 

[数据地址](https://github.com/titainic/hadoop/blob/master/com-dl4j/src/main/resources/point1.csv){: .btn .btn-green }

</div>
```markdown
1.204442292254319	3.5761141481288448	0
0.15870990398255372	-1.4821709003683443	0
0.09524719604502052	-1.2799553276531797	0
0.3491784746396591	-2.064379974990181	0
0.6941503784609095	2.8891087790998613	0
1.6206503790381426	-4.035124003739587	0
1.5385622498851343	3.242555178757121	0
0.03630856406497362	-0.8539528551255181	0
0.4745911086621599	2.477938198083137	0
0.16569582775413008	1.8111721624523829	0
1.6644624883674752	3.4527004226520135	0
0.8402857203773455	-3.440176377396835	0
0.2616951627197809	2.1054964720237748	0
0.23161489612332622	1.813805014626099	0
1.580130195201237	3.302671143882371	0
0.006355099499450956	0.418131769061616	0
0.6806104186276647	3.0963643364659914	0
...
...
...
```

---

{: .highlight }
平面视图(第一列为x轴数据，第二列为y轴数据。第三列为数据类型0,1) 

![](../../../assets/images/dl/001.png)

---

{: .highlight }
浅层神经网络的总体设计图：

![](../../../assets/images/dl/002.jpg)

---

{: .important-title }
> 神经网络
>
> 正向转播

---

{: .highlight }
首选400条数据，经过4个神经节点，如下图：

![](../../../assets/images/dl/003.jpg)

---

> {: .highlight }
矩阵化计算，矩阵化重构 成$$400 \times 3$$的数据矩阵，乘以 $$3 \times 4$$的权重矩阵，在经过矩阵化的Sigmoid，就是400条数据经过4个神经节点以后得到的值：$$A_1$$

$$Sigmoid(X^1_{400\times3} \times W^1_{3\times4})= \frac {1} {1+e^{-x_1 \times w_1}}=A_1$$

---

> {: .highlight }
展开如下：

$$Sigmoid  
\left(
\left[
\begin{matrix}
x^1_1 & x^2_1 & 1 \\
x^1_2 & x^2_2 & 1 \\
∶ & ∶ & ∶ \\
x^1_{400} & x^2_{400} & 1
\end{matrix}
\right] 

\times

\left[
\begin{matrix}
w^1_1 & w^1_2 & w^1_3 & w^1_4\\
w^2_1 & w^2_2 & w^2_3 & w^2_4\\
w^b_1 & w^b_2 & w^b_3 & w^b_4
\end{matrix}
\right]

= Z^1_{400 \times 4}
\right)
= A_1
$$

---

{: .highlight }
其中把$$w^b_1$$可以看成偏置项b,细节展开如下：

$$ Sigmoid(x^1_1  w^1_1 + x^2_1  w^2_1 + 1 \times w^b_1 = Z_1 )=A^1_1$$

---

{: .important-title }
> 计算方式
>
> 如此矩阵计算的方式，就可以一次性计算出400条数据，每条数据2个特征，经过4个神经节点以后的值
>
> 计算出来A1之后到输出层，矩阵化重构成 $$1 \times 5$$ 的权重矩阵，乘以$$400 \times 5$$的数据矩阵



设计图如下：
{: .label .label-green }

![](../../../assets/images/dl/004.jpg)

---

{: .highlight }
矩阵化数据重构如下计算：


$$Sigmoid(W^2_{1\times5} \times X^2_{5\times400})= \frac {1} {1+e^{-w_2 \times x_2}}   = A_2$$

---

{: .highlight }
矩阵展开如下：

$$Sigmoid  
\left(
\left[
\begin{matrix}
w_1 \\
w_2 \\
w_3 \\
w_4 \\
w_b
\end{matrix}
\right]

\times

\left[
\begin{matrix}
x^1_{A_1} & x^1_{A_2} & x^1_{A_3} & x^1_{A_4} & 1 \\
x^2_{A_1} & x^2_{A_2} & x^2_{A_3} & x^2_{A_4} & 1 \\
x^3_{A_1} & x^3_{A_2} & x^3_{A_3} & x^3_{A_4} & 1 \\
∶     &    ∶  &   ∶   &   ∶   &   ∶  \\
x^{400}_{A_1} & x^{400}_{A_2} & x^{400}_{A_3} & x^{400}_{A_4} & 1 \\
\end{matrix}
\right]

= Z^2_{1 \times 400}
\right)
= A_2
$$

---

{: .highlight }
细节展开如下：

$$ Sigmoid(w_1 x^1_{A_1} + w_2 x^1_{A_2} + w_3 x^1_{A_3} + w_4 x^1_{A_4} + w_b = Z_1 )=A^1_2$$

{: .highlight }


{: .note-title }
> 细节
>
> 其中$$w_b$$可以看做是偏置项b

---

{: .highlight }
输出层：

![](../../../assets/images/dl/005.jpg)

---

{: .highlight }
输出层损失函数为：

$$ Loss = \frac 1 N \sum_{i=1}^n (y_i - \hat y_i)^2 $$ 

---

{: .important-title }
> 神经网络
>
> 反向传播：

{: .highlight }
正向传播数学形式可以看做:

$$ Loss = \frac 1 N \sum_{i=1}^n (y - sigmoid(sigmoid( x w_1 ) \times w_2))^2 $$

---

{: .highlight }
完全展开如下：:

$$ Loss = \frac 1 N \sum_{i=1}^n (y_i - \frac {1} {1+e^{-w_2 \times { \frac {1} {1+e^{-w_1 x_1}}} }}) $$

---

{: .warning }
> 梯度下降的权重，都是相对于损失函数而言。 根据损失函数的误差，进行W1,W2的梯度下降



{: .highlight }
然后对这个复合函数根据链式求导法则，求w1,w2的梯度，进行梯度下降

---

{: .highlight }
$$W_2$$的梯度下降权重更新如下:

$$
\begin{align}
\hat y  & = A_2  = \frac 1 {1+e^{-w_2 x_2}} \\
\\
Loss & = \frac 1 N \sum_{i=1}^m (y_i - A_{2i})^2 = \frac 1 N \sum_{i=1}^m (y_i - \frac {1} {1+e^{-w_2 x_2}})^2\\
\\
\frac {\partial L} {\partial W_2} & = \frac {\partial L} {\partial A_2} \times \frac {\partial A_2} {\partial Z_2} \times \frac {\partial Z_2} {\partial W_2} \\
\\
\frac {\partial L} {\partial A_2} & = \frac 1 N \sum_{i=1}^m (y_i - A_{2})^2 = - \frac 2 N \sum_{i=1}^m (y_i - A_{2}) \\
\\
\frac {\partial A_2} {\partial Z_2}  & = \frac 1 {1+e^{-z_2}} = \frac 1 {1+e^{-z_2}} \times  ( 1 - \frac 1 {1+e^{-z_2}} ) \\
\\
\frac {\partial Z_2} {\partial W_2} & = W_2 \times X_2 = X_2 \\
\\
\frac {\partial L} {\partial W_2} & = \frac {\partial L} {\partial \hat y} \times \frac {\partial \hat y} {\partial Z_2} \times \frac {\partial Z_2} {\partial W_2} \\
\\
& =  - \frac 2 N \sum_{i=1}^m(y_i - \hat y_i)  \times \frac 1 {1+e^{-z_2}} \times  ( 1 - \frac 1 {1+e^{-z_2}} ) \times  X_2 \\
W_2 & = W_2 - \frac {\partial L} {\partial W_2} \times α = [1 \times 5] - [1 \times 400 ] \times [1 \times 400 ] \times [1 \times 400 ] \times [5 \times 400 ]^T \times α \\
\end{align}
$$

---

{: .highlight }
$$W_1$$的梯度下降权重更新如下:

$$
\begin{align}
\frac {\partial L} {\partial W_1} & = \frac {\partial L} {\partial A_2} \times \frac {\partial A_2} {\partial Z_2} \times \frac {\partial Z_2} {\partial A_1} \times \frac {\partial A_1} {\partial Z_1} \times \frac {\partial Z_1} {\partial W_1}  \\
\\
\frac {\partial L} {\partial A_2} & =  \frac 1 N \sum_{i=1}^m(y_i - \hat y_i)^2 = - \frac 2 N \sum_{i=1}^m(y_i - \hat y_i)    \qquad [1 \times 400] \\
\\
\frac {\partial A_2} {\partial Z_2} & = \frac 1 {1+e^{-z_2}} = \frac 1 {1+e^{-z_2}} \times  ( 1 - \frac 1 {1+e^{-z_2}} ) \qquad [1 \times 400]  \times [1 \times 400] \\
\\
\frac {\partial Z_2} {\partial A_1} & = W_2 \times A_1 =W_2  \qquad [1 \times 400] \\
\\
\frac {\partial A_1} {\partial Z_1} & = \frac 1 {1+e^{-z_1}} = \frac 1 {1+e^{-z_1}} \times  ( 1 - \frac 1 {1+e^{-z_1}} ) \qquad [400 \times 4]  \times [400 \times 4] \\
\\
\frac {\partial Z_1} {\partial W_1} & = X_1 \times W_1 = X_1  \qquad [400 \times 3] \\
\\
\frac {\partial L} {\partial W_1} & =\frac {\partial L} {\partial D_2} \times \frac {\partial D_2} {\partial Z_2} \times  \frac {\partial Z_2} {\partial D_1} \times \frac {\partial D_1} {\partial Z_1} \times \frac {\partial Z_1} {\partial W_1} \\
\\
& =  - \frac 2 N \sum_{i=1}^m(y_i - \hat y_i)  \times \frac 1 {1+e^{-z_2}} \times  ( 1 - \frac 1 {1+e^{-z_2}} ) \times  W_2 \times  \frac 1 {1+e^{-z_1}} \times  ( 1 - \frac 1 {1+e^{-z_1}} ) \times  X_1 \\
\\

\end{align} 
$$

$$
\begin{align}
[1 \times 400]^T \times [1 \times 400]^T \times [1 \times 400]^T \times [1 \times 4] \times [400 \times 4] \times [400 \times 4]  = [400 \times 4] \\
\\
[400 \times 3]^T [400 \times 4] = [3 \times 4] \\
\\
W_1 = W_1 - \frac {\partial L} {\partial W_1} \times α \\
\end{align}
$$

---

{: .warning }
> 注意点乘和叉乘的区别 .点乘为Hadamard 乘积

{: .highlight }
反向传播算法基于常规的线性代数运算——诸如向量加法，向量矩阵乘法等。但是有一个运算不大常见。特别地，假设 s和 t 是两个同样维度的向量。那么我们使用 s⊙t 来表示按元素的乘积。所以 s⊙t 的元素就是 (s⊙t)j=sjtj。示例如下：

$$
\left[
\begin{matrix}
1\\
2
\end{matrix}
\right]

\times

\left[
\begin{matrix}
3\\
4
\end{matrix}
\right] 

=

\left[
\begin{matrix}
1 \times 3\\
2 \times 4
\end{matrix}
\right]

= 

\left[
\begin{matrix}
3\\
8
\end{matrix}
\right]
$$

{: .highlight }
这种类型的按元素乘法有时候被称为 Hadamard 乘积或者 Schur 乘积。我们这里取前者。好的矩阵库通常会提供 Hadamard 乘积的快速实现，在实现反向传播的时候用起来很方便。

---

{: .highlight }
反向传播代码如下：



```
public static Map<String, INDArray> bp(INDArray X, INDArray Z2, INDArray W2, INDArray Y, INDArray yHat, INDArray Z1, INDArray X1, INDArray W1, INDArray A1)
{
Map<String, INDArray> map = new HashMap<>();

        double N = Y.size(0);

        //w2的梯度更新
        INDArray DL_DA = Y.transpose().sub(yHat).mul(2 / N).mul(-1);
        INDArray DA_DZ2_1 = sigmoidForINDArray(Z2);
        INDArray DA_DZ2_2 = sigmoidForINDArray(Z2).rsub(1);
        INDArray partialW2 = DL_DA.mul(DA_DZ2_1).mul(DA_DZ2_2).mmul(X.transpose());
        INDArray newW2 = W2.sub(partialW2.mul(learningrate));

        //w1的梯度更新
        INDArray tmpW2 = W2.getColumns(0, 1, 2, 3);
        INDArray DL_DA1 = DL_DA.mul(DA_DZ2_1).mul(DA_DZ2_2).transpose().mmul(tmpW2);
        INDArray DA_DZ1_1 = sigmoidForINDArray(Z1);
        INDArray DA_DZ1_2 = sigmoidForINDArray(Z1).rsub(1);
        INDArray DL_DZ1 = DL_DA1.mul(DA_DZ1_1).mul(DA_DZ1_2);
        INDArray partialW1 = X1.transpose().mmul(DL_DZ1);
        INDArray newW1 = W1.sub(partialW1.mul(learningrate));

        map.put("W2", newW2);
        map.put("W1", newW1);

        return map;
    }
   
```

---

{: .highlight }
训练50000次，学习率为0.1.神经网络的平面化视图为 ：

![](../../../assets/images/dl/006.jpg)

---

[代码地址](https://github.com/titainic/hadoop/tree/master/com-dl4j){: .btn .btn-green .mr-4 }

---

{: .highlight }
以下是使用DL4J神经网络框架构建的浅层神经

```
MultiLayerConfiguration conf = new NeuralNetConfiguration
                .Builder()
                .seed(123456)
                .weightInit(WeightInit.XAVIER)
                .optimizationAlgo(OptimizationAlgorithm.LINE_GRADIENT_DESCENT)
                .updater(new Sgd(0.1))
                .list()
                .layer(0,new DenseLayer.Builder()
                        .nIn(2)
                        .nOut(4)
                        .activation(SIGMOID)
                        .build())
                .layer(1,new OutputLayer
                        .Builder(LossFunctions.LossFunction.MSE)
                        .nIn(4)
                        .nOut(1)
                        .activation(SIGMOID).build())
                .build();

        MultiLayerNetwork net = new MultiLayerNetwork(conf);

        net.init();
```

---

{: .highlight }
使用tablesaw视图如下：

![](../../../assets/images/dl/007.jpg)



