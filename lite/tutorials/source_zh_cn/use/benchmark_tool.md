# Benchmark工具

<!-- TOC -->

- [Benchmark工具](#benchmark工具)
    - [概述](#概述)
    - [环境准备](#环境准备)
    - [参数说明](#参数说明)
    - [使用示例](#使用示例)
        - [性能测试](#性能测试)
        - [精度测试](#精度测试)

<!-- /TOC -->

<a href="https://gitee.com/mindspore/docs/blob/master/lite/tutorials/source_zh_cn/use/benchmark_tool.md" target="_blank"><img src="../_static/logo_source.png"></a>

## 概述

Benchmark工具是一款可以对MindSpore Lite模型进行基准测试的工具，由C++语言编码实现。它不仅可以对MindSpore Lite模型前向推理执行耗时进行定量分析（性能），还可以通过指定模型输出进行可对比的误差分析（精度）。

## 环境准备

使用Benchmark工具，需要进行如下环境准备工作。

- 编译：Benchmark工具代码在MindSpore源码的`mindspore/lite/tools/benchmark`目录中，参考部署文档中的[环境要求](https://www.mindspore.cn/lite/docs/zh-CN/master/deploy.html#id2)和[编译示例](https://www.mindspore.cn/lite/docs/zh-CN/master/deploy.html#id5)，安装编译依赖基本项，并执行编译。

- 运行：参考部署文档中的[输出件说明](https://www.mindspore.cn/lite/docs/zh-CN/master/deploy.html#id4)，获得`benchmark`工具，并配置环境变量。

## 参数说明

使用编译好的Benchmark工具进行模型的基准测试时，其命令格式如下所示。

```bash
./benchmark --modelPath=<MODELPATH> [--accuracyThreshold=<ACCURACYTHRESHOLD>]
			[--calibDataPath=<CALIBDATAPATH>] [--cpuBindMode=<CPUBINDMODE>]
			[--device=<DEVICE>] [--help] [--inDataPath=<INDATAPATH>]
			[--inDataType=<INDATATYPE>] [--loopCount=<LOOPCOUNT>]
			[--numThreads=<NUMTHREADS>] [--omModelPath=<OMMODELPATH>]
			[--resizeDims=<RESIZEDIMS>] [--warmUpLoopCount=<WARMUPLOOPCOUNT>]
```

下面提供详细的参数说明。

| 参数名            | 属性 | 功能描述                                                     | 参数类型                                                 | 默认值 | 取值范围 |
| ----------------- | ---- | ------------------------------------------------------------ | ------ | -------- | ---------------------------------- |
| `--modelPath=<MODELPATH>` | 必选 | 指定需要进行基准测试的MindSpore Lite模型文件路径。 | String | null  | -        |
| `--accuracyThreshold=<ACCURACYTHRESHOLD>` | 可选 | 指定准确度阈值。 | Float           | 0.5    | -        |
| `--calibDataPath=<CALIBDATAPATH>` | 可选 | 指定标杆数据的文件路径。标杆数据作为该测试模型的对比输出，是该测试模型使用相同输入并由其它深度学习框架前向推理而来。 | String | null | - |
| `--cpuBindMode=<CPUBINDMODE>` | 可选 | 指定模型推理程序运行时绑定的CPU核类型。 | Integer | 1      | -1：表示中核<br/>1：表示大核<br/>0：表示不绑定 |
| `--device=<DEVICE>` | 可选 | 指定模型推理程序运行的设备类型。 | String | CPU | CPU、NPU、GPU |
| `--help` | 可选 | 显示`benchmark`命令的帮助信息。 | - | - | - |
| `--inDataPath=<INDATAPATH>` | 可选 | 指定测试模型输入数据的文件路径。如果未设置，则使用随机输入。 | String | null | - |
| `--inDataType=<INDATATYPE>` | 可选 | 指定测试模型输入数据的文件类型。 | String | bin | img：表示输入数据的文件类型为图片<br>bin：表示输入数据的类型为二进制文件 |
| `--loopCount=<LOOPCOUNT>` | 可选 | 指定Benchmark工具进行基准测试时，测试模型的前向推理运行次数，其值为正整数。 | Integer | 10 | - |
| `--numThreads=<NUMTHREADS>` | 可选 | 指定模型推理程序运行的线程数。 | Integer | 2 | - |
| `--omModelPath=<OMMODELPATH>` | 可选 | 指定OM模型的文件路径，此参数仅当`device`类型为NPU时可选设置。 | String | null | - |
| `--resizeDims=<RESIZEDIMS>` | 可选 | 指定测试模型输入数据需要调整的尺寸大小。 | String | null | - |
| `--warmUpLoopCount=<WARMUPLOOPCOUNT>` | 可选 | 指定测试模型在执行基准测试运行轮数前进行的模型预热推理次数。 | Integer | 3 | - |

## 使用示例

对于不同的MindSpore Lite模型，在使用Benchmark工具对其进行基准测试时，可通过设置不同的参数，实现对其不同的测试功能。主要分为性能测试和精度测试。

### 性能测试

Benchmark工具进行的性能测试主要的测试指标为模型单次前向推理的耗时。在性能测试任务中，不需要设置`calibDataPath`等标杆数据参数。例如：

```bash
./benchmark --modelPath=./models/face/age/ml_face_age.ms
```

这条命令使用随机输入，其他参数使用默认值。该命令执行后会输出如下统计信息，该信息显示了测试模型在运行指定推理轮数后所统计出的单次推理最短耗时、单次推理最长耗时和平均推理耗时。

```
Model = ml_face_age.ms, numThreads = 2, MinRunTime = 72.228996 ms, MaxRuntime = 73.094002 ms, AvgRunTime = 72.556000 ms
```

### 精度测试

Benchmark工具进行的精度测试主要是通过设置标杆数据来对比验证MinSpore模型输出的精确性。在精确度测试任务中，除了需要设置`modelPath`参数以外，还必须设置`calibDataPath`参数。例如：

```bash
./benchmark --modelPath=./models/face/age/ml_face_age.ms --inDataPath=./data/input/1_3_120_120_NCHW.bin --device=NPU --accuracyThreshold=3 --calibDataPath=./data/output/face/ml_face_age.out
```

这条命令指定了测试模型的输入数据、标杆数据，同时指定了模型推理程序在NPU上运行，并指定了准确度阈值为3%。该命令执行后会输出如下统计信息，该信息显示了测试模型的单条输入数据、输出节点的输出结果和平均偏差率以及所有节点的平均偏差率。

```
InData0: 139.947 182.373 153.705 138.945 108.032 164.703 111.585 227.402 245.734 97.7776 201.89 134.868 144.851 236.027 18.1142 22.218 5.15569 212.318 198.43 221.853
================ Comparing Output data ================
Data of node age_out : 5.94584e-08 6.3317e-08 1.94726e-07 1.91809e-07 8.39805e-08 7.66035e-08 1.69285e-07 1.46246e-07 6.03796e-07 1.77631e-07 1.54343e-07 2.04623e-07 8.89609e-07 3.63487e-06 4.86876e-06 1.23939e-05 3.09981e-05 3.37098e-05 0.000107102 0.000213932 0.000533579 0.00062465 0.00296401 0.00993984 0.038227 0.0695085 0.162854 0.123199 0.24272 0.135048 0.169159 0.0221256 0.013892 0.00502971 0.00134921 0.00135701 0.000383242 0.000163475 0.000136294 9.77864e-05 8.00793e-05 5.73874e-05 3.53858e-05 2.18535e-05 2.04467e-05 1.85286e-05 1.05075e-05 9.34751e-06 6.12732e-06 4.55476e-06
Mean bias of node age_out : 0%
Mean bias of all nodes: 0%
=======================================================
```