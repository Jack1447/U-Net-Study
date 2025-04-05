# 一. 复现 U-Net

## 📌 基本信息

- **复现依据**：[Mastering U-Net: A Step-by-Step Guide to Segmentation from Scratch with PyTorch](https://medium.com/@fernandopalominocobo/mastering-u-net-a-step-by-step-guide-to-segmentation-from-scratch-with-pytorch-6a17c5916114)
- **训练平台**：Kaggle  
  - 优点：便于下载数据集；自带 GPU/TPU；环境预配置完善；适合快速实验  
  - 缺点：运行环境是临时的，不能长期保存  
- **数据集**：carvana-image-masking-challenge  
  - 图片转化为三通道张量  
  - 分割掩码为单通道张量，尺寸相同，标注像素类别
- **目标**：使用 U-Net 自动识别二维图像中汽车的边界

---

## 🛠 实现过程

### 1. 加载并处理数据集

需要了解数据集的：
- 图像类型（普通图像 or 医学 MRI）
- 通道数
- 尺寸
- 标注格式是否对齐
- 期望输出

### 2. 搭建 U-Net 架构

- **输入/输出尺寸匹配**
- **基本块设计**：
  - 卷积层：尺寸减小，通道数增加 → 特征提取  
  - 池化：获取 `p` 作为下一层输入  
- **瓶颈层**：高层特征融合，跳跃连接尺寸匹配
- **解码路径**：
  - 反卷积：恢复尺寸
  - 跳跃连接：补充细节
  - 卷积融合：合并上下文信息
- **深度设计注意**：
  - 通道数常以 2 倍增长
  - 特征图不能太小
  - 可考虑 Patch-based 改进

### 3. 优化器与损失函数

- **AdamW 优化器**：自适应学习率，结合一阶与二阶梯度信息，使更新更稳定
- **BCEWithLogitsLoss**：适合二分类（带 Sigmoid 激活），结合交叉熵和 logits 输出

---

## 🧪 实验流程

- **评估指标**：DICE 系数
- **训练步骤**：
  - 读取数据
  - 清零梯度
  - 前向传播（计算损失与 DICE）
  - 反向传播（更新参数）
  - 验证与绘图观察损失与 DICE 变化趋势
- **测试阶段**：评估最终模型性能

---

## 📈 评价

- 已基本掌握 U-Net 实现流程
- 对 U-Net 架构理解尚浅，尤其对各层作用的感性认识
- AdamW + BCEWithLogitsLoss 的理论理解有待加强
- 下周目标：复现 Task01_BrainTumour（MRI），重点了解预处理与代码编写