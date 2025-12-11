> 进迭时空Demo Zoo集成多个优化模型与开箱即用方案，覆盖多种应用场景。本文列举进迭时空Demo Zoo模型列表，可供开发者参考。

# 1. CV模型列表

| 模型名称           | 模型文件路径                                              | 模型类型 | 量化版本 |
| :----------------- | :-------------------------------------------------------- | :------- | :------- |
| EfficientNet-B0    | efficientnet/models/efficientnet_v1_b0.q.onnx             | 分类     | int8     |
| EfficientNet-B1    | efficientnet/models/efficientnet_v1_b1.q.onnx             | 分类     | int8     |
| FCN-R50            | fcn/model/fcn_r50.q.onnx                                  | 语义分割 | int8     |
| Inception-V1       | inception_v1/models/inception_v1.q.onnx                   | 分类     | int8     |
| Inception-V3       | inception_v3/models/inception_v3.q.onnx                   | 分类     | int8     |
| MobileNet-V2       | mobilenet_v2/models/mobilenet_v2.q.onnx                   | 分类     | int8     |
| ResNet18           | resnet/models/resnet18.q.onnx                             | 分类     | int8     |
| ResNet50           | resnet/models/resnet50.q.onnx                             | 分类     | int8     |
| Swin-Tiny          | swin-tiny_16xb64_in1k/models/swin-tiny_16xb64_in1k.q.onnx | 分类     | int8     |
| UNet-S5            | unet/model/unet-s5.q.onnx                                 | 语义分割 | int8     |
| YOLOv11n           | yolov11/model/yolo11n.q.onnx                              | 目标检测 | int8     |
| YOLOv11n (320x320) | yolov11/model/yolo11n_320x320.q.onnx                      | 目标检测 | int8     |
| YOLOv5n            | yolov5/model/yolov5_n.onnx                                | 目标检测 | float32  |
| YOLOv5n            | yolov5/model/yolov5_n.q.onnx                              | 目标检测 | int8     |
| YOLOv6n            | yolov6/model/yolov6p5_n.q.onnx                            | 目标检测 | int8     |
| YOLOv8n            | yolov8/model/yolov8n.q.onnx                               | 目标检测 | int8     |
| YOLOv8s            | yolov8/model/yolov8s_320x320.onnx                         | 目标检测 | float32  |

模型和测试代码打包链接：[CV模型支持列表](code/spacemit_demo/examples/CV)