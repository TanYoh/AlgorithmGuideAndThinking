# GBDT和XGBoost的不同

1. 一介导和二阶导；
2. GBDT的弱分类器为CART，XGBoost既可为CART，也可为LR等；
3. GBDT的特征划分切法依赖于基尼系数，而XGBoost依赖于自己的增益函数；
4. XGBoost运用了很多工程优化的方法：包括并行列块设计、缓存访问和核外块计算
