# SYSZUXface
DeepVAC-face test dataset.
一个高质量的用于1:N人脸识别的开源人脸测试集。

SYSZUXface具有如下特点：

- 1:N的人脸识别；
- ds划分为不同的子分支，方便以后的扩展；
- db划分为不同的子分支，方便灵活组合；
- 以中国人为主，未来也会兼顾其它人种；
- 提供原始图片，检测和识别均需用户自定义实现，更贴近产业；

dataset目录中的图片文件使用git lfs维护，克隆该项目前，你需要首先安装git-lfs：
```bash
#on Linux
apt install git-lfs

#on macOS
brew install git-lfs
```
然后：
```bash
#克隆该项目
git clone https://github.com/DeepVAC/SYSZUXface

#拉取dataset图片
git lfs pull
```

## 使用说明
定义如下概念：
- TP: 预测结果大于阈值，且正确；也即已注册db的ds 匹配到了 正确的db；
- FP: 预测结果大于阈值，且错误；也即没注册db的ds 匹配到了 某个db，或者已注册db的ds匹配到了错误的db；
- TN: 预测结果小于阈值，且正确；也即没注册db的ds 没有匹配 任何db；
- FN: 预测结果小于阈值，且错误；也即已注册db的ds 没有匹配 任何db；

定义准确率、精确率、召回率如下：
- 准确率(accuracy) = (TP+TN)/(TP+FP+TN+FN)
- 精确率(precision) = TP/(TP+FP)
- 召回率(recall) = TP/(TP+FN)
- 漏检率(漏检率) = FN/(TP+FP+TN+FN)
- 错误率(错误率) = (FP+FN)/(TP+FP+TN+FN)

项目的目录说明如下：
|  目录   |  说明   |
|---------|---------|
|dataset  |数据集   |
|src     |测试示例代码|
|dataset/db | 底库图片，包含多个子目录，默认合并使用|
|dataset/db/famous | 底库图片，ds/famous测试图片对应的底库图片|
|dataset/db/soccer | 底库图片，ds/soccer测试图片对应的底库图片|
|dataset/db/allage | 底库图片，底库的干扰项图片|
|dataset/db/hancheng | 底库图片，底库的干扰项图片|
|dataset/db/manyi | 底库图片，底库的干扰项图片|
|dataset/ds | 待测试图片，包含多个子目录 |
|dataset/ds/famous | 待测试图片，以公众人物为主，对应的底库图片为db/famous |
|dataset/ds/soccer | 待测试图片，以足球运动员为主，对应的底库图片为db/soccer |

经典的测试步骤如下：
- 合并dataset/db下的子目录，组成大而全的底库，目前具备4w+的底库图片；
- 调整自己的人脸检测和识别算法，确保每个每个底库图片都能提取出特征；
- 使用自定义人脸检测和识别算法，生成适用于自己算法的底库；
- 使用自定义人脸检测和识别算法，对ds的子目录进行检测、特征提取、底库特征匹配；
- 计算ds/soccer的准确率、精确率、召回率、漏检率、错误率；
- 计算ds/famous的准确率、精确率、召回率、漏检率、错误率；

上述数据可以通过deepvac项目lib库的syszux_report模块来简化计算，下面是个示例：
```python
#use the FaceReport class
from deepvac import FaceReport
#total 5 images to test in gemfield dataset
report = FaceReport('gemfield',5)
#add 5 predict records
report.add("","2").add("gemfield","gemfield").add(1,1).add(None,None).add("1","1")
#report.add(None,None)
#report.add(None,"1")
#report.add("1",None)
#report.add("1","1")
#report.add("1","2") #FP
report()
```
程序会输出markdown格式的报告：
```bash
|dataset|total|duration|accuracy|precision|recall|miss|error|
|--|--|--|--|--|--|--|--|
|gemfield|5|0.000|0.8|0.75|1.0|0.0|0.2|
```
放入项目的md文件中，在web上会显示为：
|dataset|total|duration|accuracy|precision|recall|miss|error|
|--|--|--|--|--|--|--|--|
|gemfield|5|0.000|0.8|0.75|1.0|0.0|0.2|

## 使用许可
本项目仅限用于纯粹的学术研究，如：
- 个人学习；
- 比赛排名；
- 公开发表且开源其实现的论文；

不得用于任何形式的商业牟利，包括但不限于：
- 任何形式的商业获利行为；
- 任何形式的商务机会获取；
- 任何形式的商业利益交换；


## 项目贡献
我们欢迎各种形式的贡献，包括但不限于：
- 提交自己的作品/产品在SYSZUXface上的成绩；
- 发现和Fix项目的bug；
- 提交高质量的测试集数据；

## RetinaFace项目：
RetinaFace项目实现了人脸检测，对齐，识别（包含制作底库），打印report的端到端流程，可以作为参考。
具体情况请参考[RetinaFace](https://github.com/DeepVAC/RetinaFace)
