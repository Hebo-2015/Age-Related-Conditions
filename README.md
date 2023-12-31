# Age-Related-Conditions
这个项目是一个使用机器学习算法来预测年龄相关疾病的分类问题。

# 数据集
数据集来自于Kaggle上的一个竞赛，它包含了六百多个个样本，包括五十多个与三种年龄相关的状况相关的匿名健康特征。标签是一个二元变量，表示样本是否患有年龄相关疾病。

# 实现过程
## 1.首先查看数据分布情况
![image](https://github.com/Hebo-2015/Age-Related-Conditions/assets/142399606/679989c5-1ba8-418c-9744-79e6748ded2b)

## 2.数据清洗
- 对数据集进行一些预处理和标准化

- 对数据集中的一些特征进行交互（interaction），即将两个特征相乘，并将结果存储在新的数据框newF和newF_test中。目的是扩充样本数量，提取深层特征。

- 使用IV来选择最佳变量，并用WoE对每个特征进行转换。信息值(Information Value, IV)和证据权重(Weights of evidence, WoE)被广泛应用于logistic回归的信用评分模型中。IV是一个双变量度量，它对变量相对于目标的预测能力进行排序。WoE将数据转换为单调模式，这是逻辑回归模型良好运行的基础。
  - 计算方式
      首先，将自变量（X）和目标变量（Y）分别分组，例如对连续型变量进行分箱，对离散型变量进行分类。每个分组称为一个箱（bin） 。
      然后，计算每个箱中正例（Y=1）和负例（Y=0）的数量和比例，分别记为Ni+​，Ni−​，Pi+​，Pi−​。其中i表示第i个箱 。
      接着，计算每个箱的WOE值，即正负例比例之比的自然对数，公式为：
  
      WOEi​=lnPi−​Pi+​​
      
      WOE值反映了每个箱中正负例的相对分布情况。如果WOE值为正，则表示该箱中正例占比较高；如果WOE值为负，则表示该箱中负例占比较高；如果WOE值为0，则表示该箱中正负例占比相同 。
  
      最后，计算每个箱的IV部分值，即正负例比例之差乘以WOE值，公式为：
  
      IVi​=(Pi+​−Pi−​)×WOEi​
      
      然后将所有箱的IV部分值相加，得到整个变量的IV值，公式为：
      
      IV=i=1∑n​IVi​
    
    其中n表示箱的个数。IV值反映了变量对目标变量的预测能力。一般来说，IV值越大，表示变量越重要；IV值越小，表示变量越无关 。

## 3.挑选相关性更高的特征用于后续的训练

# 使用四种不同的机器学习算法：逻辑回归（LR），随机森林（RF），XGBoost（XGB）和支持向量机（SVM）进行预测

|模型 | 损失值 | 训练得分 | 预测得分 |
|--- |---|---|---|
|LR|0.1893868728837159|0.9522779830189901|0.9432575356953996|
|SVM|0.2419118801254251|0.9928689480847752|0.9459175039661555|
|XGB|0.49072009009756407|1.0|0.9209307244843999|
|RF|0.4559277989240336|1.0|0.9099365415124276|

# 结论
通过四种模型训练结果对比，可以发现支持向量机与逻辑回归模型对此数据集的拟合效果最好，逻辑回归的测试损失之最小。由于使用WoE将数据转换为单调模式，这对逻辑回归模型有一定的优化，因此逻辑回归的预测效果优于随机森林和XGBoost。后期可以考虑针对随机森林和XGBoost进行一定的优化，以提高预测效果。

