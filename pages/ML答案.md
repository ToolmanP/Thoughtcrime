- F T T T F T T F T T
  logseq.order-list-type:: number
- (1) CBOW KNN KMeans VAE
  logseq.order-list-type:: number
- (2) 较底层：偏词本意在词义空间上
	- self-attention:  使用attention上对文章上下文的语义进行凝练
- (3) 这个可以类比RNN的循环神经网络，在较前的输入由于时间序列的影响导致梯度消失，导致无法记忆较前的输入层，导致记忆衰退。这对应汉字拼读在时间序列中，目前大多数人读得多导致语言模型基于记忆到目前的拼读。同时由于训练集中qi的人数增加，导致训练时对qi过拟合。
- (5) 迁移学习，比如大语言模型调参，例如bert，bert通过在通用语料上的进行训练，然后在进行翻译任务中通过将特定的双语语料进行输入，得到翻译任务的知识。又或者在visual-transformer中通过使用region-aware network以及特征提取器将特征翻译成sequence再利用预训练后的transformer调参，则能做到图像caption生成。
- 4.
- (1)
- (2)