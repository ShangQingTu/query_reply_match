贝壳找房

# 数据

6000段对话

# 流程

- 数据预处理
  - preprocess.py
  - 找规则
- 训练
  - train.py
- 模型
  - model.py
  - 采用ALBERT+TextCNN

## 结果

```
python train.py --batch_size 32 --lr 1e-4 --weight_decay 0
```

![](/home/tsq/Downloads/小学生/my_beike/深度截图_选择区域_20201022160527.png)

发现，已经快过拟合了。。

所以从这里面选一个ckpt就行，剩下的看运气了，如果和测试集的分布一样就分高。

## 验证

用了规则兜底，如果不行，还可以人工check 

```
python test.py --ckpt xxx --thres xx  --use_rule
```

```
python test.py --ckpt xxx --thres xx
```



```
python test.py --ckpt model_epoch4_val0.978.pt --thres 0.15
```

![](/home/tsq/Downloads/小学生/my_beike/错误/kwargs.png)

由于kwargs参数保留少了,之前很傻逼的只保存了'max_input_len'，还自以为是地把所有参数args输入到model里做初始化。

现在终于知道为什么，大家写model的__init__函数要把参数都列出来了，因为ckpt加载的时候，加载出kwargs要全部重新输入到model的__init__函数里面

现在把model的__init__函数改了，重新训练:

```
python train.py --batch_size 32 --lr 1e-4 --weight_decay 0 --max_epoch 10
```



最后得分是0.7

