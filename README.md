民航旅客航程状态判断
===================
# 数据处理模块
## 数据预处理
### 1.去掉所有非迁徙的非闭环行程串； 
### 2. 把行程串切割成航段生成航段表，每一条记录格式形如(user, order, dst, dpt, time)
### 运行命令：python data_preprocessing.py -c=True -r=../raw_data/itny/raw_itny_100k.csv
 -  -c  判断数据集是否需要进行预处理，默认为False
    -r  Json数据路径，行程串文件路径
### 3. csv文件导入postgresql数据库

# 特征提取模块
## 从postgresql中读取用户行程记录， 生成训练集与测试集
## python iagnn-demo\feature_extraction\DataGenerate.py
# 模型训练模块
## CUDA_VISIBLE_DEVICE=id python train.py --aux_path="../raw_data/" --cuda=id -tb=128 -pb=128 -me=30 -lr=1e-3 -hs=100 --l2=1e-5 --lr_dc=0.5 --lr_dc_step=10 --step=2 --is_restore
-   --cuda  使用cuda的id，一般与CUDA_VISIBLE_DEVICES指定id相同
    --aux_path  数据集存储路径
    -m          模型名字，默认为ggnn
    -v          是否用validation set，默认为False
    -tb         训练batch_size
    -pb         预测batch_size
    -me          最大训练epoch
    -lr         学习率
    -hs         中间层向量维度,默认为100
    --l2        正则化参数
    --step      gnn传播步数
    --nonhyrbid 是否不用全局偏好，默认为False
    --lr_dc     学习率衰减参数
    --lr_dc_step    学习率衰减轮数
    --is_restore  是否调用已有模型，默认为false
