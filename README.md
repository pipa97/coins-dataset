# Coins Dataset (YOLO)

## 关于这个数据集

这是我训练 YOLO11n 用的硬币训练集和测试集，图片全部从我自己拍摄的视频中提取帧得到，之后由大肥鱼整理了一下，开源了出来。大家可以直接拿去训练模型，如果有问题，欢迎提交 Issue 来「拷打」我。

这个数据集有点小，建议你自己再拍一些图片作为补充。另外要注意，我用的硬币年份分别是：一元 1994、五角 2022、一角 2021。拿不同年份的硬币参与训练，也许能提高模型的泛化能力（大概）。

---

三分类硬币目标检测数据集，YOLO 标注格式，可直接用于 YOLOv5 / YOLOv8 / YOLOv11 等框架训练。

## 数据集概览

| 项目 | 数值 |
|---|---|
| 类别数 | 3 |
| 训练集图片 | 196 |
| 验证集图片 | 49 |
| 合计图片 | 245 |
| 标注文件 | 245（与图片一一对应） |
| 图片格式 | JPG |
| 标注格式 | YOLO txt（`class x_center y_center width height`，归一化到 0~1） |
| 数据集体积 | 约 161 MB |

## 类别

| ID | 类别名 | 说明 |
|---|---|---|
| 0 | wujiao | 五角 |
| 1 | yijiao | 一角 |
| 2 | yiyuan | 一元 |

## 目录结构

```
coins-dataset/
├── images/
│   ├── train/          # 196 张训练图片
│   └── val/            # 49 张验证图片
├── labels/
│   ├── train/          # 196 个训练标签 + classes.txt
│   └── val/            # 49 个验证标签 + classes.txt
├── classes.txt         # 类别名列表
├── data.yaml           # YOLO 数据集配置
├── .gitattributes      # Git LFS 规则
└── README.md
```

## 标注格式说明

每个 `.txt` 与同名图片对应，每行一个目标：

```
<class_id> <x_center> <y_center> <width> <height>
```

坐标均已归一化到 `[0, 1]`。示例（`labels/train/00_0000.txt`）：

```
1 0.385117 0.721768 0.174063 0.169935
```

## 使用方法

### 1. 克隆仓库（需要 Git LFS）

```bash
git lfs install
git clone https://github.com/pipa97/coins-dataset.git
```

> 图片通过 Git LFS 存储，克隆前务必先安装 Git LFS，否则只能拿到指针文件。

### 2. 用 YOLO 训练

`data.yaml` 中的 `path` 字段需改为你本地的数据集路径：

```yaml
path: /your/path/to/coins-dataset
train: images/train
val: images/val

names:
  0: wujiao
  1: yijiao
  2: yiyuan
```

以 Ultralytics YOLOv8 为例：

```python
from ultralytics import YOLO

model = YOLO("yolov8n.pt")
model.train(data="data.yaml", epochs=100, imgsz=640)
```

## 数据划分

- 训练集与验证集按图片划分，同一图片的图片文件与标签文件始终成对出现在同一 split 中。
- 训练集 196 张 / 验证集 49 张，比例约 8:2。
- 所有图片均有对应标注，无空白标签、无孤立文件。

## 文件命名

图片与标签统一使用 `00_XXXX` 格式（如 `00_0000.jpg` / `00_0000.txt`），
train 与 val 各自从 `00_0000` 独立编号：

- `images/train` 与 `labels/train`：`00_0000` ~ `00_0195`
- `images/val` 与 `labels/val`：`00_0000` ~ `00_0048`

## 来源与许可

本数据集由数据集所有者自行采集与整理，拥有公开再分发的权利。

如用于研究或商业项目，建议在成果中注明数据集来源。

## 已知限制

- 数据集规模较小（245 张），建议配合数据增强或预训练权重使用。
- 类别分布、场景多样性有限，跨域泛化能力需自行验证。
- 标注质量未经过第三方复核，如有问题欢迎提 Issue。

## 引用

如本项目对你的研究有帮助，可参考：

```
Coins Dataset (YOLO), 3-class coin object detection dataset.
https://github.com/pipa97/coins-dataset
```
