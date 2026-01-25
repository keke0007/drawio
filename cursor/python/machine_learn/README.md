# 机器学习案例与数据资源

## 📚 目录
- [分类案例](#分类案例)
- [回归案例](#回归案例)
- [聚类案例](#聚类案例)
- [深度学习案例](#深度学习案例)
- [自然语言处理案例](#自然语言处理案例)
- [计算机视觉案例](#计算机视觉案例)
- [推荐系统案例](#推荐系统案例)
- [数据资源](#数据资源)

---

## 分类案例

### 1. 鸢尾花分类 (Iris Classification)
- **问题类型**: 多分类问题
- **数据集**: Iris数据集（150个样本，4个特征，3个类别）
- **算法**: KNN、决策树、SVM、随机森林
- **数据来源**: sklearn内置数据集
- **应用场景**: 入门级分类问题，特征工程基础

### 2. 手写数字识别 (MNIST)
- **问题类型**: 多分类问题（0-9数字识别）
- **数据集**: MNIST（60000训练样本，10000测试样本，28x28像素）
- **算法**: 逻辑回归、SVM、CNN、随机森林
- **数据来源**: 
  - sklearn: `fetch_openml('mnist_784')`
  - TensorFlow: `tf.keras.datasets.mnist`
  - PyTorch: `torchvision.datasets.MNIST`
- **应用场景**: 图像分类入门

### 3. 垃圾邮件分类 (Spam Detection)
- **问题类型**: 二分类问题
- **数据集**: 
  - UCI Spam Base数据集
  - Enron邮件数据集
  - 自定义邮件数据
- **算法**: 朴素贝叶斯、逻辑回归、SVM、随机森林
- **数据来源**: 
  - UCI ML Repository
  - Kaggle
- **应用场景**: 文本分类、特征提取

### 4. 乳腺癌诊断 (Breast Cancer Detection)
- **问题类型**: 二分类问题（良性/恶性）
- **数据集**: Wisconsin Breast Cancer数据集（569个样本，30个特征）
- **算法**: 逻辑回归、SVM、决策树、XGBoost
- **数据来源**: sklearn内置数据集
- **应用场景**: 医疗诊断、特征重要性分析

### 5. 信用卡欺诈检测 (Credit Card Fraud Detection)
- **问题类型**: 二分类问题（不平衡数据集）
- **数据集**: 
  - Kaggle信用卡欺诈数据集（284807条交易）
  - 特征已做PCA处理
- **算法**: 逻辑回归、随机森林、XGBoost、异常检测
- **数据来源**: Kaggle
- **应用场景**: 异常检测、不平衡数据处理

### 6. 情感分析 (Sentiment Analysis)
- **问题类型**: 二分类/多分类（正面/负面/中性）
- **数据集**: 
  - IMDB电影评论（50000条）
  - Twitter情感分析数据集
  - 中文情感分析数据集
- **算法**: 朴素贝叶斯、LSTM、BERT、TextCNN
- **数据来源**: 
  - Kaggle
  - Hugging Face Datasets
- **应用场景**: 文本情感分析、NLP入门

### 7. 泰坦尼克号生存预测 (Titanic Survival)
- **问题类型**: 二分类问题
- **数据集**: Titanic数据集（891个样本，12个特征）
- **算法**: 逻辑回归、决策树、随机森林、XGBoost
- **数据来源**: Kaggle入门竞赛
- **应用场景**: 特征工程、数据清洗、EDA

---

## 回归案例

### 1. 房价预测 (House Price Prediction)
- **问题类型**: 回归问题
- **数据集**: 
  - Boston Housing（506个样本，13个特征）
  - California Housing（20640个样本，8个特征）
  - Kaggle房价预测竞赛数据
- **算法**: 线性回归、岭回归、Lasso、随机森林、XGBoost
- **数据来源**: 
  - sklearn内置数据集
  - Kaggle
- **应用场景**: 回归分析、特征工程

### 2. 股票价格预测 (Stock Price Prediction)
- **问题类型**: 时间序列回归
- **数据集**: 
  - 历史股价数据（Yahoo Finance、Alpha Vantage）
  - 自定义股票数据
- **算法**: ARIMA、LSTM、Prophet、XGBoost
- **数据来源**: 
  - yfinance库
  - Alpha Vantage API
  - Kaggle
- **应用场景**: 时间序列分析、金融预测

### 3. 广告点击率预测 (CTR Prediction)
- **问题类型**: 回归/分类问题
- **数据集**: 
  - Criteo数据集（4500万条广告展示）
  - Avazu数据集
- **算法**: 逻辑回归、FM、DeepFM、XGBoost
- **数据来源**: Kaggle、Criteo
- **应用场景**: 推荐系统、在线广告

### 4. 能源消耗预测 (Energy Consumption)
- **问题类型**: 时间序列回归
- **数据集**: 
  - 电力消耗数据
  - 温度、湿度等环境数据
- **算法**: ARIMA、LSTM、Prophet、XGBoost
- **数据来源**: UCI ML Repository、Kaggle
- **应用场景**: 时间序列预测、能源管理

---

## 聚类案例

### 1. 客户细分 (Customer Segmentation)
- **问题类型**: 无监督聚类
- **数据集**: 
  - 客户购买行为数据
  - Mall Customer数据集
- **算法**: K-Means、DBSCAN、层次聚类、GMM
- **数据来源**: Kaggle、UCI
- **应用场景**: 市场营销、用户画像

### 2. 图像分割 (Image Segmentation)
- **问题类型**: 聚类问题
- **数据集**: 
  - 自然图像
  - 医学图像
- **算法**: K-Means、Mean Shift、谱聚类
- **数据来源**: 自定义图像数据
- **应用场景**: 计算机视觉、图像处理

### 3. 文档聚类 (Document Clustering)
- **问题类型**: 文本聚类
- **数据集**: 
  - 新闻文章
  - 学术论文
  - 产品评论
- **算法**: K-Means、LDA、层次聚类
- **数据来源**: 20 Newsgroups、自定义文本
- **应用场景**: 文本挖掘、主题发现

---

## 深度学习案例

### 1. 图像分类 (Image Classification)
- **问题类型**: 多分类问题
- **数据集**: 
  - CIFAR-10（10类，50000训练，10000测试）
  - CIFAR-100（100类）
  - ImageNet（大规模数据集）
- **算法**: CNN、ResNet、VGG、EfficientNet
- **数据来源**: 
  - TensorFlow: `tf.keras.datasets.cifar10`
  - PyTorch: `torchvision.datasets.CIFAR10`
- **应用场景**: 计算机视觉、图像识别

### 2. 目标检测 (Object Detection)
- **问题类型**: 目标检测
- **数据集**: 
  - COCO数据集
  - Pascal VOC
  - 自定义标注数据
- **算法**: YOLO、R-CNN、SSD、RetinaNet
- **数据来源**: COCO官网、Pascal VOC官网
- **应用场景**: 自动驾驶、安防监控

### 3. 图像生成 (Image Generation)
- **问题类型**: 生成模型
- **数据集**: 
  - CelebA（人脸数据集）
  - MNIST
  - 自定义图像数据
- **算法**: GAN、VAE、Diffusion Models
- **数据来源**: Kaggle、自定义数据
- **应用场景**: 图像生成、数据增强

### 4. 文本生成 (Text Generation)
- **问题类型**: 序列生成
- **数据集**: 
  - 小说文本
  - 新闻文章
  - 代码数据
- **算法**: RNN、LSTM、GRU、Transformer、GPT
- **数据来源**: 自定义文本数据
- **应用场景**: 文本生成、聊天机器人

---

## 自然语言处理案例

### 1. 文本分类 (Text Classification)
- **问题类型**: 多分类问题
- **数据集**: 
  - 20 Newsgroups（20个类别）
  - AG News（4个新闻类别）
  - 中文新闻分类数据集
- **算法**: 朴素贝叶斯、SVM、LSTM、BERT、TextCNN
- **数据来源**: 
  - sklearn: `fetch_20newsgroups`
  - Hugging Face Datasets
- **应用场景**: 新闻分类、文档分类

### 2. 命名实体识别 (NER)
- **问题类型**: 序列标注
- **数据集**: 
  - CoNLL-2003（英文NER）
  - MSRA（中文NER）
  - 自定义标注数据
- **算法**: CRF、BiLSTM-CRF、BERT
- **数据来源**: 
  - CoNLL官网
  - 中文NER数据集
- **应用场景**: 信息抽取、知识图谱

### 3. 机器翻译 (Machine Translation)
- **问题类型**: 序列到序列
- **数据集**: 
  - WMT数据集
  - 中英文平行语料
- **算法**: Seq2Seq、Transformer、BERT
- **数据来源**: 
  - WMT竞赛数据
  - OPUS语料库
- **应用场景**: 翻译系统、多语言处理

### 4. 问答系统 (Question Answering)
- **问题类型**: 阅读理解
- **数据集**: 
  - SQuAD（Stanford Question Answering Dataset）
  - 中文QA数据集
- **算法**: BERT、RoBERTa、GPT
- **数据来源**: 
  - SQuAD官网
  - Hugging Face Datasets
- **应用场景**: 智能客服、知识问答

---

## 计算机视觉案例

### 1. 人脸识别 (Face Recognition)
- **问题类型**: 人脸识别/验证
- **数据集**: 
  - LFW（Labeled Faces in the Wild）
  - CelebA
  - 自定义人脸数据
- **算法**: FaceNet、ArcFace、Eigenfaces
- **数据来源**: 
  - LFW官网
  - Kaggle
- **应用场景**: 身份验证、安防系统

### 2. 图像风格迁移 (Style Transfer)
- **问题类型**: 图像生成
- **数据集**: 
  - 自然图像
  - 艺术作品
- **算法**: Neural Style Transfer、GAN
- **数据来源**: 自定义图像数据
- **应用场景**: 艺术创作、图像处理

### 3. 图像超分辨率 (Super Resolution)
- **问题类型**: 图像增强
- **数据集**: 
  - DIV2K数据集
  - 自定义低分辨率图像
- **算法**: SRCNN、ESRGAN、Real-ESRGAN
- **数据来源**: DIV2K官网
- **应用场景**: 图像修复、视频增强

---

## 推荐系统案例

### 1. 电影推荐 (Movie Recommendation)
- **问题类型**: 协同过滤、内容推荐
- **数据集**: 
  - MovieLens（100K/1M/10M/20M）
  - Netflix Prize数据集
- **算法**: 协同过滤、矩阵分解、深度学习
- **数据来源**: 
  - MovieLens官网
  - GroupLens Research
- **应用场景**: 个性化推荐、电商推荐

### 2. 商品推荐 (Product Recommendation)
- **问题类型**: 推荐系统
- **数据集**: 
  - Amazon Product Data
  - 电商平台数据
- **算法**: 协同过滤、FM、DeepFM、Wide&Deep
- **数据来源**: 
  - Amazon公开数据
  - 自定义电商数据
- **应用场景**: 电商推荐、个性化营销

---

## 数据资源

### 1. 官方数据集库

#### Python库内置数据集
- **sklearn.datasets**: 
  - `load_iris()` - 鸢尾花
  - `load_boston()` - 波士顿房价（已弃用，使用`fetch_california_housing()`）
  - `load_breast_cancer()` - 乳腺癌
  - `load_digits()` - 手写数字
  - `fetch_20newsgroups()` - 新闻分类
  - `fetch_openml()` - OpenML数据集

- **TensorFlow/Keras**:
  - `tf.keras.datasets.mnist` - MNIST
  - `tf.keras.datasets.cifar10` - CIFAR-10
  - `tf.keras.datasets.cifar100` - CIFAR-100
  - `tf.keras.datasets.imdb` - IMDB电影评论

- **PyTorch**:
  - `torchvision.datasets.MNIST`
  - `torchvision.datasets.CIFAR10`
  - `torchvision.datasets.ImageNet`

#### Hugging Face Datasets
```python
from datasets import load_dataset
dataset = load_dataset("dataset_name")
```
- 包含大量NLP、CV、音频等数据集
- 网址: https://huggingface.co/datasets

### 2. 公开数据集平台

#### Kaggle
- **网址**: https://www.kaggle.com/datasets
- **特点**: 
  - 大量真实世界数据集
  - 包含竞赛数据
  - 社区活跃
- **热门数据集**:
  - Titanic
  - House Prices
  - Credit Card Fraud Detection
  - Sentiment Analysis

#### UCI Machine Learning Repository
- **网址**: https://archive.ics.uci.edu/
- **特点**: 
  - 经典机器学习数据集
  - 学术研究常用
  - 数据质量高
- **热门数据集**:
  - Iris
  - Wine
  - Adult (Census Income)
  - Spam Base

#### Google Dataset Search
- **网址**: https://datasetsearch.research.google.com/
- **特点**: 数据集搜索引擎

#### Papers with Code
- **网址**: https://paperswithcode.com/datasets
- **特点**: 论文相关数据集

### 3. 特定领域数据集

#### 计算机视觉
- **ImageNet**: https://www.image-net.org/
- **COCO**: https://cocodataset.org/
- **Pascal VOC**: http://host.robots.ox.ac.uk/pascal/voc/
- **CIFAR**: https://www.cs.toronto.edu/~kriz/cifar.html

#### 自然语言处理
- **GLUE**: https://gluebenchmark.com/
- **SQuAD**: https://rajpurkar.github.io/SQuAD-explorer/
- **20 Newsgroups**: sklearn内置
- **IMDB**: TensorFlow/Keras内置

#### 时间序列
- **UCI Time Series**: UCI Repository
- **M4 Competition**: https://www.m4.unic.ac.cy/
- **Yahoo Finance**: yfinance库

#### 推荐系统
- **MovieLens**: https://grouplens.org/datasets/movielens/
- **Amazon Product Data**: https://nijianmo.github.io/amazon/index.html
- **Netflix Prize**: https://www.netflixprize.com/

### 4. 数据获取工具

#### Python库
```python
# 金融数据
import yfinance as yf
data = yf.download("AAPL", start="2020-01-01")

# 网络爬虫
import requests
from bs4 import BeautifulSoup

# 数据库连接
import pandas as pd
df = pd.read_sql("SELECT * FROM table", connection)

# API调用
import requests
response = requests.get("API_URL")
```

### 5. 数据生成工具

#### 合成数据生成
```python
# sklearn生成数据
from sklearn.datasets import make_classification, make_regression
X, y = make_classification(n_samples=1000, n_features=20)

# 时间序列生成
from statsmodels.tsa.arima_process import ArmaProcess
```

---

## 📝 使用建议

### 初学者推荐
1. **Iris分类** - 理解分类问题
2. **Boston Housing** - 理解回归问题
3. **MNIST** - 图像分类入门
4. **Titanic** - 完整项目流程

### 进阶学习
1. **信用卡欺诈检测** - 不平衡数据处理
2. **情感分析** - NLP入门
3. **客户细分** - 无监督学习
4. **推荐系统** - 实际应用

### 高级项目
1. **图像生成** - GAN/VAE
2. **机器翻译** - Seq2Seq/Transformer
3. **目标检测** - YOLO/R-CNN
4. **强化学习** - OpenAI Gym

---

## 🔗 快速链接

- [Kaggle数据集](https://www.kaggle.com/datasets)
- [UCI ML Repository](https://archive.ics.uci.edu/)
- [Hugging Face Datasets](https://huggingface.co/datasets)
- [Papers with Code](https://paperswithcode.com/)
- [Google Dataset Search](https://datasetsearch.research.google.com/)

---

## 📚 学习路径

1. **基础阶段**: Iris、Boston Housing、MNIST
2. **进阶阶段**: 情感分析、客户细分、推荐系统
3. **高级阶段**: 深度学习、NLP、CV项目
4. **实战阶段**: Kaggle竞赛、实际业务项目

---

*最后更新: 2026-01-25*
