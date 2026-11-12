# Fashion-MNIST 换色分类实验

[English](#english)

衣服的轮廓一样，换个底色和衣服颜色，分类器还认得出来吗？这个机器学习小课设用 Fashion-MNIST 做一次可控的测试，看看颜色变化会让准确率掉多少，以及在训练时随机换色能补回多少。

## 怎么测

训练图是二值化后的黑底白图。两套测试都用原始测试集里的 10,000 张图，类别和轮廓相同。黑白测试（同分布，ID）沿用训练集的配色，换色测试（分布外，OOD）给每张图的衣物和背景分别随机上色。

用灰度像素训练随机森林作对照。两个 CNN 用同一种网络结构，一个只看黑白图，另一个在训练时给每批图片随机换色。

[黑白测试样例](experiment/figures/samples_test_id.png) · [换色测试样例](experiment/figures/samples_test_ood.png)

## 实验结果

仓库里的结果来自一次完整运行，CNN 训练 8 轮，随机森林用 200 棵树。

| 方法 | 黑白测试准确率 | 换色测试准确率 |
| --- | ---: | ---: |
| 随机森林 | 83.63% | 27.59% |
| CNN | 86.88% | 16.10% |
| CNN + 随机换色 | 82.19% | 84.09% |

加了随机换色后，CNN 在换色测试上提高了 67.99 个百分点，在黑白测试上下降了 4.69 个百分点。

[准确率对照图](experiment/figures/summary.png) · [逐类报告](experiment/results/best_classification_report.txt)

## 本地运行

从 Zalando 下载四个原始 `.gz` 文件，原样放到 `data/fashion/`。[下载数据][data] · [数据集许可](data/fashion/LICENSE)

建议使用 Python 3.12。在仓库根目录运行以下命令。

```bash
python -m pip install -r experiment/requirements.txt
python experiment/main.py
```

先试跑可以用 `python experiment/main.py --smoke`，CNN 训练 1 轮，随机森林用 30 棵树。每次运行会更新 `experiment/results/` 的指标和 `experiment/figures/` 的图表。

---

## English

Can a classifier still recognize an item of clothing after its colors change? This machine learning course project uses Fashion-MNIST to measure how much a change in color hurts accuracy and how much random recoloring during training can recover.

### Setup

The training images are binarized to white silhouettes on black backgrounds. Both tests use the same 10,000 images and keep the original silhouettes and labels. The in-distribution (ID) test keeps the black-and-white palette. The out-of-distribution (OOD) test assigns a random color to the clothing and another to the background.

A random forest trained on grayscale pixels serves as a reference. The two CNNs use the same architecture. One trains on black-and-white images and the other recolors each batch during training.

[Black-and-white test samples](experiment/figures/samples_test_id.png) · [Recolored test samples](experiment/figures/samples_test_ood.png)

### Results

These scores are from one full run with 8 CNN training epochs and 200 trees in the random forest.

| Method | Black-and-white test accuracy | Recolored test accuracy |
| --- | ---: | ---: |
| Random forest | 83.63% | 27.59% |
| CNN | 86.88% | 16.10% |
| CNN + random recoloring | 82.19% | 84.09% |

With recoloring during training, the CNN gained 67.99 percentage points on the recolored test and lost 4.69 points on the black-and-white test.

[Accuracy chart](experiment/figures/summary.png) · [Per-class report](experiment/results/best_classification_report.txt)

### Run locally

Download the four `.gz` files from Zalando and place them in `data/fashion/` as-is. [Dataset files][data] · [Dataset license](data/fashion/LICENSE)

Use Python 3.12 and run these commands from the repository root.

```bash
python -m pip install -r experiment/requirements.txt
python experiment/main.py
```

For a shorter run, use `python experiment/main.py --smoke`. It trains each CNN for one epoch and uses 30 trees in the random forest. Each run updates the scores in `experiment/results/` and plots in `experiment/figures/`.

[data]: https://github.com/zalandoresearch/fashion-mnist/tree/master/data/fashion
