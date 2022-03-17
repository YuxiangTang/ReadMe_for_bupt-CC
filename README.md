### 文件名

Func_interface.py: 实现了一些函数接口，供传音调用。

example.py：针对函数接口，实现了一些调用Func_interface.py中接口的相关实例，供传音参考。

### 接口函数介绍

以下数组类型均为numpy库数组：np.ndarray类型。



- def ProcessRaw(raw_data:np.ndarray, whitelevel=1024, blacklevel=64) 

> 上述函数中参数解读==》冒号":"后接的是变量类型，等号"="后接的是默认值

```
功能：

​ 对RAW数据进行预处理：包括去马赛克、去黑白电平。（一般传音用不到，北邮做AWB前的处理）

参数：

​ raw_data：RAW数据，数组类型，一维数据。一般通过np.fromfile(path, dtype=np.uint16)读取。

​ whitelevel：饱和像素，int类型。

​ blacklevel：黑电平，int类型。

返回：

​ RGB图像，数组类型，H×W×3。数据范围[0，1024×16]
```



- def PureBackground(img:np.ndarray, model_path = "./MDCC/Pureback/workplace/train_all_random.pkl")   

```
功能：

​ 对给定的图像进行场景分类。纯色场景调用北邮AWB模型，非纯色场景调用传音AWB模型。

参数：

​ img：经过去马赛克、去黑白电平后的rawRGB数据。数组类型，H×W×3。

​ model_path：分类模型的路径。str类型。

​ 目前demo提供的可供选择的模型有:

    ​ 在CE9上训练的SVM模型，路径./MDCC/Pureback\workplace\train_ce9.pkl；

	​ 在CE9上训练的随机森林模型，路径./MDCC/Pureback\workplace\train_ce9_random.pkl；

	​ 在x687上训练的SVM模型，路径./MDCC/Pureback\workplace\train_x687.pkl；

	​ 在x687上训练的随机森林模型，路径./MDCC/Pureback\workplace\train_x687_random.pkl；

	​ 在所有训练集上训练的SVM模型，路径./MDCC/Pureback\workplace\train_all.pkl；

	​ 在所有训练集上训练的随机森林模型，路径./MDCC/Pureback\workplace\train_all_random.pkl；

返回：

​ 1 或者 0。1为：纯色场景，调用北邮AWB；0：非纯色场景，调用传音的AWB。
```

​		

3. def <font color=#FF0000>get_Awb_CCM(img:np.ndarray, ccm_mode = True, awbmodel_path='./MDCC/pretrained/transsion_old.pth', device='cpu')</font>

```
功能：

​ 对图像进行AWB和颜色校正，返回估计的RGB增益值和CCM转换矩阵。

参数：

​ img：经过去马赛克、去黑白电平后的raw-RGB数据。数组类型，H×W×3。

​ ccm_mode：指定是否返回CCM转换矩阵。Bool类型：True时返回3×3CCM矩阵 or False时返回None类型。

​ awbmodel_path：AWB模型的路径，字符串类型。默认即可。

​ device：用于指定运行环境为cpu还是gpu，字符串类型。“cpu”、“cuda:0”、“cuda:1”等

返回：

​ 用于AWB的RGB增益值，数组类型，一维向量。

​ 用于颜色校正的CCM转换矩阵，数组类型，3×3 矩阵。
```



4. def img_gamma(img:np.ndarray)

```
功能：

​ 对图像进行gamma矫正，并将结果映射至[0,255]，用作代码调试时图片的展示。（一般传音用不到，北邮调试过程中为了展示图片而写）

参数：

​ img：gamma矫正前的图像，数组类型，H×W×3。

返回：

​ 经过gamma矫正后的图像，数组类型，uint8类型，H×W×3，数据范围[0,255]。
```


