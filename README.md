# DeepLearning NER for bishe

### 本文结构包括：
1. 工程内容。
2. 工程设计。
3. 工程结构。  

* * *

### 工程内容：利用BILSTM+CRF 完成命名实体的提取（主要针对人名、地名、组织机构）

### 工程设计：
* 数据处理
   * 数据存放：输入数据（文本、数据库、命令行）、输出数据（同上）
   * 数据映射：W2I和I2W以及L2I和I2L的建立及存放调用,词向量来源(FUDAN DNN)。
   * 数据格式：
      1. INPUT:字+' '+label作为一行，句间空格隔开。
      2. LABEL:采用BIES（开始，中间，结尾，单个）+ PLOO（PER,LOC,ORG，O）
      3. OUTPUT:同输入。
   * 数据划分:
      1. Version1.
         + 训练集: 100%(FUDAN DNN) + 1000及以上(警情数据)。
         + 开发集：100-1000(警情数据)。
         + 测试集：所有警情数据。
      2. TODO
   * 数据后期处理：将OUTPUT转换成(标签)+(实体)。
   * TODO
   
* 模型层:
   1. Token Embedding
      * 输入：单个字。
      * 转换成索引。
      * 输出：查表得对应的词向量。
      
   2. BILSTM
      * 输入：sentence对应的token embedding(num_steps,emb_dim)
      * 经过双向LSTM训练
      * 输出：两个lstm输出的向量v1,v2的拼接作为标签特征(num_steps,v1+v2)
      
   3. 标签预测
      * 输入：标签特征
      * 经过softmax多分类器
      * 输出：标签对应概率
      
   3. CRF
      * 输入：标签对应概率
      * 经过转移矩阵和发射条件综合
      * 输出：最优序列
   
   4. 模型评判标准
      * PRECISION
      * RECALL
      * F1
            
* 需要的插件及包
   + TENSORBOARD
   + sql相关
   + tensorflow
   + csv
   + 序列化(pickle)
   + time
   + numpy
   + re
   + argparse?
   + collection?
   + math?

### 工程结构：
* data
   + input data(folder)
      - test(folder)
         + test batch(file)
      - train(folder)
         + train batch(file)
      - dev(folder)
         + dev batch(file)
      - metadata(file)
      - test(file)
      - train(file)
      - dev(file)
      
   + word_vectors(folder)
      + w2v(file)
      
   + parameters(folder)
      - parameter(file) (各种配置选项)
* output
   + outputTime(folder)
      - model(folder)
      - tensorboard logs(folder)
      - result(file) (可能是多种结果展示)

* src
   + main.py(file) (主入口) --4
   + train.py(file) (训练) --2
   + test.py(file) (测试) --5
   + dataset.py(file) (数据处理) --3
   + model.py(file) (模型)    --1
   + prepare_pretrained_model(file) (加载模型) --7
   + helper.py(file) (插件) --x
   + evaluate.py(file) (评估) --6

* trained_models
   + xxxmodel(folder)
   
---

## Others:TODO
