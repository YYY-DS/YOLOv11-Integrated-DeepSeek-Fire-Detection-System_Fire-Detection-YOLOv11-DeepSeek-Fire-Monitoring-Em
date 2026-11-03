# YOLOv11 融合DeepSeek火灾检测系统  | 火灾检测 YOLOv11 DeepSeek 消防监控 应急响应9169期

## 系统概述

本系统基于 **YOLOv11 深度学习模型**与 **DeepSeek 大语言模型**，融合 **Pytorch、Flask、SpringBoot、Vue3、MySQL** 等技术，构建了一套支持**图片、视频、摄像头**三种检测方式的火灾检测与预警平台。系统适配消防监控、应急响应及公共安全等场景。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0c601c5f7506454694992e83155a308d.png)

## 系统核心信息

| 项目 | 详细说明 |
| :--- | :--- |
| **检测类别** | 共 **1** 类：`火焰` |
| **核心技术** | YOLOv11 + DeepSeek + Pytorch + Flask + SpringBoot + Vue3 |
| **检测模式** | 图片检测、视频检测、摄像头实时检测 |
| **系统架构** | 前后端分离（Vue3 + SpringBoot + Flask + MySQL） |
| **应用价值** | 1. **消防监控**：实时监测火焰，实现火灾早期预警。<br>2. **应急响应**：快速定位火点，辅助消防力量部署。<br>3. **公共安全**：用于公共场所、工业区域的火灾监控与预警。 |


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1de0aef49ad54a50b3a200fb17fe627e.png)

## 技术栈

| 层次 | 技术选型 |
| :--- | :--- |
| **前端** | Vue3、Element-Plus、TypeScript |
| **后端** | SpringBoot、MyBatis-Plus、Flask |
| **深度学习** | YOLOv11、Pytorch |
| **数据库** | MySQL |
| **视频处理** | FFmpeg |
| **AI 分析** | DeepSeek |


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5302c4e60d5d412a8d91b0dde32627f5.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bd9017ca215242d0ba931fbd623b5fc3.png)

## 系统功能

**1. 多方式检测**
- **图片检测**：支持单张图片上传检测
- **视频检测**：上传视频文件，逐帧分析并输出标注视频
- **摄像头实时检测**：连接摄像头，实时输出检测结果

**2. 火灾检测识别**
- 支持火焰检测，精准高效
- 使用 YOLOv11 模型，支持多种权重的目标检测
- 检测精度高、速度快

**3. 实时监控与预测**
- 支持通过摄像头进行实时监测
- 自动识别火灾并触发预警

**4. 模型训练与自定义**
- 用户可以使用自定义数据集重新训练模型
- 生成新的权重文件，支持 YOLOv11 的训练

**5. 简洁美观的 UI**
- 采用 Vue3 前端框架
- 支持主题颜色、布局和组件大小的个性化定制
- 兼容深色模式与色弱模式
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a057c49160cb4d4eb91f966196c1e639.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/645da07685be456aa7d4f90294e625d7.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d0b7f05348dc44c291fb3abe63d0be0d.png)

## 快速开始（代码示例）

**1. 数据集配置文件 (`data.yaml`)**
```yaml
train: ./dataset/images/train
val: ./dataset/images/val
nc: 1
names: ['fire']
```

**2. YOLOv11 训练代码**
```python
from ultralytics import YOLO

model = YOLO('yolo11n.pt')
model.train(
    data='data.yaml',
    epochs=100,
    imgsz=640,
    batch=16,
    device=0
)
```

**3. Flask 推理服务**
```python
from flask import Flask, request, jsonify
from ultralytics import YOLO
import cv2
import numpy as np

app = Flask(__name__)
model = YOLO('runs/detect/train/weights/best.pt')

@app.route('/detect', methods=['POST'])
def detect():
    file = request.files['image']
    img = cv2.imdecode(np.frombuffer(file.read(), np.uint8), cv2.IMREAD_COLOR)
    results = model(img)
    detections = []
    for box in results[0].boxes:
        detections.append({
            'class': model.names[int(box.cls[0])],
            'confidence': float(box.conf[0]),
            'bbox': box.xyxy[0].tolist()
        })
    return jsonify(detections)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**4. Vue3 前端检测页面**
```vue
<template>
  <div class="detector-container">
    <el-upload action="/api/detection/image" :on-success="handleSuccess">
      <el-button type="primary">上传图片</el-button>
    </el-upload>
    <div v-if="resultImage" class="result-area">
      <img :src="resultImage" alt="Detection Result" />
      <el-table :data="detections" border>
        <el-table-column prop="class" label="类别" />
        <el-table-column prop="confidence" label="置信度" />
      </el-table>
    </div>
  </div>
</template>
```

## 标签
`火灾检测` `YOLOv11` `DeepSeek` `消防监控` `应急响应` `目标检测` `深度学习` `计算机视觉`
