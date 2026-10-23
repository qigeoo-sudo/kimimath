---
标题: HSV Complex Plane Plot
来源: https://www.kimi.com/chat/19f9243e-87b2-8720-8000-09fe188c9079
抓取日期: 2026-10-22
---

## 用户

import numpy as np
import matplotlib.pyplot as plt
# 1. 创建复平面网格
# 在 -2 到 2 之间生成 800x800 的网格点
x = np.linspace(-2, 2, 800)
y = np.linspace(-2, 2, 800)
X, Y = np.meshgrid(x, y)
Z = X + 1j * Y # 组合成复数矩阵 z = x + iy
# 2. 计算复变函数 w = z^2
W = Z**2
# 3. 提取 w 的相位（角度）和模长
# np.angle 返回 -π 到 π 之间的角度
phase = np.angle(W)
magnitude = np.abs(W)
# 4. 将极坐标转换为 HSV 颜色空间，以便完美可视化
# H (Hue): 用相位表示颜色（0~1 对应 0~2π）
# S (Saturation): 饱和度设为 1
# V (Value): 用模长表示亮度（为了不让无穷远处全白，我们做个对数压缩）
H = (phase + np.pi) / (2 * np.pi) # 将 -π~π 映射到 0~1
S = np.ones_like(H)
V = np.log1p(magnitude) / np.log1p(2) # 对数压缩，让远处的颜色也能看清
V = np.clip(V, 0, 1) # 限制在 0~1 之间
# 组合 HSV 并转换为 RGB
HSV = np.stack((H, S, V), axis=-1)
# matplotlib 需要 RGB 格式，这里用颜色空间转换
from matplotlib.colors import hsv_to_rgb
RGB = hsv_to_rgb(HSV)
# 5. 绘图
plt.figure(figsize=(8, 8))
plt.imshow(RGB, extent=[-2, 2, -2, 2])
# 添加坐标轴和标题
plt.axhline(0, color='white', linewidth=0.5)
plt.axvline(0, color='white', linewidth=0.5)
plt.title(r'Phase Portrait of $w = z^2$ (复平面相图)', fontsize=15)
plt.xlabel(r'Re(z)', fontsize=12)
plt.ylabel(r'Im(z)', fontsize=12)
# 显示网格
plt.grid(True, color='white', linestyle='--', linewidth=0.3, alpha=0.5)
plt.show()

## Kimi

运行并评估 w=z^2 相位图代码

用户分享了一段绘制复变函数 w=z2 相位图的 Python 代码，并希望我运行它或提供反馈。这段代码看起来是完整的，可以直接运行。我应该：

1. 运行代码生成图像
2. 对代码进行简要分析和点评

让我先用 ipython 运行这段代码。

运行 Python 代码

[图片: 20260724T035612079386Z-ipython-tmp-1b094b19d64d4034a6b9e33bc2b5140b.png]

复平方映射相位图显示零点与相位加倍

代码已经成功运行，生成了 w=z2 的相位图。图像非常清晰地展示了复平方映射的特性：

1. **原点处的零点**：在 z=0 处， w=0 ，所以模长为0，亮度为0（黑色），这是二阶零点。
2. **相位加倍**：复平方映射将角度加倍。可以看到颜色（相位）在原点周围旋转了两圈，对应 w=z2 将 z 平面的角度 θ 映射到 w 平面的角度 2θ 。
3. **模长平方**： ∣w∣=∣z∣2 ，所以远离原点时模长增长很快，对数压缩后可以看到亮度梯度。
4. **对称性**：由于 z2 是偶函数，图像关于原点对称。

中文字体有警告，但不影响图像主体。让我给用户一个清晰的反馈。用户可能只是分享代码让我运行，或者想让我点评。我应该：

1. 展示运行结果
2. 对图像进行解读
3. 对代码做简要点评

代码运行成功！下面是生成的 w=z2 复平面相图：[sandbox:///mnt/agents/output/phase_portrait_z2.png](sandbox:///mnt/agents/output/phase_portrait_z2.png)

---

### 图像解读

这张图非常直观地展示了 w=z2 的几何特性：

| 特征 | 图中表现 |
| --- | --- |

| **二阶零点** | 原点处为纯黑色，因为 $ | w | =0$，亮度 V=0 |
| --- | --- | --- | --- |
| **角度加倍** | 绕原点一周，颜色（相位）变化两圈，对应 arg(w)=2arg(z) |
| **偶函数对称** | 图像关于原点中心对称， z 和 −z 映射到同一个 w |
| **模长平方** | 远离原点时亮度快速增加，对数压缩后呈现出清晰的径向梯度 |

### 代码点评

你的实现非常规范，几个亮点：

- **HSV 空间映射**很巧妙：色相表示相位、亮度表示模长，这是复变函数相图的标准做法
- **`np.log1p`** 做对数压缩很合适，避免了 ∣w∣→∞ 时亮度过曝
- **`np.clip`** 限制亮度范围，保证颜色有效性

一个小建议：如果加上 **等相位线**（contour of constant phase）或 **等模线**（contour of constant magnitude），可以进一步突出映射的解析结构。例如：

Python 复制

```python
# 叠加等模线（|z^2| = const）
ax.contour(X, Y, magnitude, levels=10, colors='white', linewidths=0.4, alpha=0.6)
```

整体来说，这是一段清晰、完整的复变函数可视化代码！