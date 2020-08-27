
训练流程（按顺序）：
- train_ner.py 根据语料集反向标注问题，训练一个NER模型
- mention_extractor.py: 同时使用jieba分词和bert序列标注来抽取问题中的实体mention
- prop_extractor.py: 使用规则抽取问题中的属性值
- entity_extractor.py: 将实体mention和属性值链接到知识库，得到候选实体，并为每个实体生成一些人工特征
- entity_filter.py: 根据特征，对候选实体进行逻辑回归筛选，保存逻辑回归模型
- similiary.py 根据该语料微调bert文本匹配模型；需要配置src/data/train.xls等文件。正例就是每个问题和对应的正确关系路径，负例可以随机选n个错误路径。
- tuple_extractor.py: 根据候选实体从知识库中抽取两跳内的关系作为候选答案，并根据微调后的bert文本匹配模型，生成问题+候选答案的相似度特征
- tuple_filter.py: 根据bert相似度特征对候选答案进行排序筛选
- kb.py：连接知识库，定义了一些查询函数。由于连接的是自行搭建的数据库，无法直接执行。



主要的程序文件,其中部分程序fork自 https://github.com/terrifyzhao/bert-utils 这个项目

src/data里需要配置训练文本匹配的正负例
src/chinese_L-12_H-768_A-12 放预训练的Bert中文模型
