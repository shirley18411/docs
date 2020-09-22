# CPU上推理

`Linux` `CPU` `推理应用` `初级` `中级` `高级`

<!-- TOC -->

- [CPU上推理](#cpu上推理)
    - [使用checkpoint格式文件推理](#使用checkpoint格式文件推理)
    - [使用ONNX格式文件推理](#使用onnx格式文件推理)

<!-- /TOC -->

<a href="https://gitee.com/mindspore/docs/blob/r1.0/tutorials/inference/source_zh_cn/multi_platform_inference_cpu.md" target="_blank"><img src="./_static/logo_source.png"></a>

## 使用checkpoint格式文件推理
与在Ascend 910 AI处理器上推理一样。

## 使用ONNX格式文件推理
与在GPU上进行推理类似，需要以下几个步骤：

1. 在训练平台上生成ONNX格式模型，具体步骤请参考[导出ONNX格式文件](https://www.mindspore.cn/tutorial/training/zh-CN/r1.0/advanced_use/save_load_model_hybrid_parallel.html#onnx)。

2. 在CPU上进行推理，具体可以参考推理使用runtime/SDK的文档。如使用ONNX Runtime，可以参考[ONNX Runtime说明文档](https://github.com/microsoft/onnxruntime)。