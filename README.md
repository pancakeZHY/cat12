# 猫十二分类
# 一、文件介绍
### config：设置两个网络的一些参数
### dataset: 将数据集保存解压到此处。image_list是训练集标签，后续会分割出一部分作为验证集。
### model：使用的两个模型
### output：输出结果
### dataprocess.py：处理数据集。包括清除脏数据，分割验证集


# 二、准备工作
### 将数据集和标签保存到dataset，将训练集标签命名为image_list.txt。
```
#目录结构
dataset/
├── cat_12_train
│   ├── 0aSixIFj9X73z41LMDUQ6ZykwnBA5YJW.jpg
│   ├── 0bBWRPd2t4NDIaO8567oyTgK3MU9rJZS.jpg
│   |   ...
├── cat_12_test
│   ├── mXIfNyVxBOZin4KQlYMdkPTSFA85ugrH.jpg
│   ├── zXyot03giwfhecLJlCm5NjQnY6VHq7Da.jpg
│   |   ...
|——image_list.txt
```
### 运行`python data_process.py`，清理脏数据，分割训练集和验证集。
```
#目录结构
dataset/
├── cat_12_train
│   ├── 0aSixIFj9X73z41LMDUQ6ZykwnBA5YJW.jpg
│   ├── 0bBWRPd2t4NDIaO8567oyTgK3MU9rJZS.jpg
│   |   ...
├── cat_12_test
│   ├── mXIfNyVxBOZin4KQlYMdkPTSFA85ugrH.jpg
│   ├── zXyot03giwfhecLJlCm5NjQnY6VHq7Da.jpg
│   |   ...
|——image_list.txt
|——train_list.txt
|——valid_list.txt
```


# 三、启动项目
## 3.1 克隆PaddleClas
```
git clone https://gitee.com/paddlepaddle/PaddleClas.git -b release/2.3
cd PaddleClas/
pip install --upgrade -r requirements.txt -i https://mirror.baidu.com/pypi/simple
cd ../
```

## 3.2 修改配置文件
配置文件保存在config文件夹下，根据需求修改
- config/ResNet152.yaml
- config/ViT_small_patch16_224.yaml

## 3.3 模型训练
`-c`指定配置文件路径，`-o`修改配置文件参数，加载预训练模型进行微调。
- ResNet模型训练
```
python PaddleClas/tools/train.py  -c config/ResNet152.yaml  -o Arch.pretrained=True  
```
- ViT模型训练
```
python PaddleClas/tools/train.py  -c config/ViT_small_patch16_224.yaml  -o Arch.pretrained=True  
```

## 3.4 模型评估
- ResNet模型评估
```
python PaddleClas/tools/eval.py  -c config/ResNet152.yaml  -o Global.pretrained_model=output/ResNet152/best_model  
```
- ViT模型评估
```
python PaddleClas/tools/eval.py  -c config/ViT_small_patch16_224.yaml  -o Global.pretrained_model=output/ViT_small_patch16_224/best_model  
```

## 3.5 模型预测
- ResNet模型预测
```
python PaddleClas/tools/infer.py  -c config/ResNet152.yaml  -o Infer.infer_imgs=dataset/cat_12_test  -o Global.pretrained_model=output/ResNet152/best_model  
```
- ViT模型预测
```
python PaddleClas/tools/infer.py  -c config/ViT_small_patch16_224.yaml  -o Infer.infer_imgs=dataset/cat_12_test  -o Global.pretrained_model=output/ViT_small_patch16_224/best_model  
```

