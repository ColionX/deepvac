# DeepVAC标准
DeepVAC标准是由MLab团队设立，用来定义和约束AI模型的训练、测试、验收、部署。覆盖以下内容：
- 数据集
- 代码
- 训练
- 部署

基于此的DeepVAC checklist (检查单)是项目管理中必不可少的文件。

## 数据集
数据集划分为如下四种：
|  名称      |  说明   | 和训练集分布一致？  |来源 | 维护 |
|------------|---------|---------|---------|---------|
| 训练集| 基础标注数据中划分80% ~ 90%| 是 |公开数据集/自行标注| 维护在MLab:/opt/|
| 验证集| 基础标注数据中划分10% ~ 20%| 是 |公开数据集/自行标注| 维护在MLab:/opt/|
| 测试集| 用来对模型做初步测试   | 否 |公开数据集/自行标注| 维护在MLab:/opt/|
| 验收集| 用来对模型做终极测试| 否 |业务场景中真实数据上的自行标注| 维护在MLab:/opt/|

## 代码
包含如下三个方面：
#### 使用deepvac库
代码必须基于deepvac库，且维护在MLab代码服务系统上。特别的：
- 模型定义基于deepvac.syszux_modules、deepvac.syszux_*；
- 训练、测试代码基于Deepvac类体系；
- 日志基于deepvac.syszux_log；
- 配置基于deepvac.syszux_config;
- 数据合成基于deepvac.syszux_synthesis;
- 数据增强基于deepvac.syszux_aug;
- 数据动态增强基于deepvac.syszux_executor;
- dataloader基于deepvac.syszux_loader;
- 模型性能报告基于deepvac.syszux_report;

#### 使用DeepVAC代码规范
访问：[代码规范](./code_standard.md)

#### 使用DeepVAC项目组织规范
访问：[项目组织规范](./arch.md)


## 训练
所有的模型训练默认至少包含两种：
- 模型部署目标为x86+CUDA Linux的训练;
- 模型部署目标为x86 Linux、Arm Linux、ARM Android/iOS的训练；

并且最终需要满足：
- 在验收集上的指标必须符合要求；
- 各种SOTA模型要维护在MLab:/opt/；
- 报告要记录在deepvac-product项目页上。报告格式如下： 

dataset: <验收集的名称>  
tester: <测试人员的名称>  
date: <测试日期>

|dataset|total|duration|accuracy|precision|recall|miss|error|
|--|--|--|--|--|--|--|--|
|gemfield|144|75.579|0.631944444444|1.0|0.631944444444|0.118055555556|0.368055555556|
|self|2047|1065.102|0.829018075232|1.0|0.829018075232|0.0229604298974|0.170981924768|

#### 模型部署目标为x86+CUDA Linux的训练
必须开启如下开关：
- config.script_model_dir
- config.trace_model_dir
- config.static_quantize_dir

可选开启如下开关：
- config.dynamic_quantize_dir

#### 模型部署目标为x86 Linux、Arm Linux、ARM Android/iOS的训练
必须开启如下开关：
- config.script_model_dir
- config.trace_model_dir
- config.qat_dir

可选开启如下开关：
- config.onnx_model_dir
- config.ncnn_model_dir, config.onnx2ncnn
- config.coreml_model_dir, config.coreml_preprocessing_args

## 部署方式
所有的AI产品默认必须进行这3种部署测试：x86 + CUDA Linux、x86 Linux、 Arm Android；可选这2种部署测试：Arm iOS、Arm Linux。整体部署测试说明如下：
|  部署目标  | 部署方式 | 大小 | 是否必须   | 
|------------|---------|---------|---------|
|x86 + CUDA Linux| 基于 libdeepvac cuda docker image | docker image大小 | 是|
|x86 Linux   | 基于 libdeepvac x86 docker image | docker image大小 | 是|
|ARM Android | App + libdeepvac.so | libdeepvac.so大小 | 是|
|Arm iOS     | App + libdeepvac.dylib | libdeepvac.dylib大小 | 否|
|Arm Linux   | App + libdeepvac.so | libdeepvac.so大小 | 否|

测试成功后，则：
- Docker镜像维护在ai5.gemfield.org上；
- 库维护在MLab:/opt/；

## DeepVAC checklist
| 序号 | 检查项      |  结论   |
|----|-------------|---------|
| 1 | 训练集是否维护在MLab存储上 |         |
| 2 | 测试集是否维护在MLab存储上 |         |
| 3 | 验收集是否维护在MLab存储上 |         |
| 4 | 模型定义是否复用了deepvac.syszux_modules |         |
| 5 | 模型定义是否复用了deepvac.syszux_* |         |
| 6 | loss定义是否复用了deepvac.syszux_loss |         |
| 7 | 训练代码是否基于deepvac库 |         |
| 8 | 测试代码是否基于deepvac库 |         |
| 9 | 日志代码是否基于deepvac库    |         |
|10 | 配置代码是否基于deepvac库    |         |
|11 | 数据合成代码是否基于deepvac库    |         |
|12 | 数据增强和动态增强代码是否基于deepvac库    |         |
|13 | dataloader代码是否基于deepvac库      |         |
|14 | 模型性能报告代码是否基于deepvac库    |         |
|15 | 项目代码是否符合DeepVAC代码规范     |         |
|16 | 项目目录和文件名、git分支、配置项是否符合DeepVAC项目组织规范    |         |
|17 | 项目代码、dockerfile、README.md是否维护在了MLab代码服务系统上    |         |
|18 | 项目所依赖的预训练模型是否维护在了MLab:/opt   |         |
|19 | 项目是否完成了部署目标为x86+CUDA Linux的训练     |         |
|20 | 上述训练的验收集指标是否符合要求    |         |
|21 | 上述训练输出的SOTA模型是否维护在了MLab:/opt    |         |
|22 | 上述训练的性能报告是否记录到deepvac-product     |         |
|23 | 项目是否完成了部署目标为x86 Linux、Arm Linux、ARM Android/iOS的训练     |        |
|24 | 上述训练的验收集指标是否符合要求    |         |
|25 | 上述训练输出的SOTA模型是否维护在了MLab:/opt    |         |
|26 | 上述训练的性能报告是否记录到deepvac-product     |         |
|27 | 项目是否成功进行了x86 + CUDA Linux的部署测试    |         |
|28 | 上述测试成功的镜像是否维护在了ai5.gemfield.org    |         |
|29 | 项目是否成功进行了x86 Linux的部署测试     |         |
|30 | 上述测试成功的镜像是否维护在了ai5.gemfield.org     |         |
|31 | 项目是否成功进行了Arm Android的部署测试     |         |
|32 | 上述测试成功的libdeepvac库是否维护在了MLab:/opt     |         |

