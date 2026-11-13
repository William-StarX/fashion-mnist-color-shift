# Fashion-MNIST 换色分类实验<br>Fashion-MNIST color-shift experiment

用同一批服装图片比较随机森林、普通 CNN 和加入随机换色训练的 CNN，观察颜色变化对分类结果的影响。运行一次即可生成准确率、混淆矩阵、错分样例和特征图。

Compare a random forest, a standard CNN, and a CNN trained with random recoloring on the same clothing images. One run produces accuracy scores, confusion matrices, misclassified examples, and feature plots.

## 快速开始<br>Quick start

建议使用 **Python 3.12**。先获取代码：

Use **Python 3.12** and get the code:

```sh
git clone https://github.com/William-StarX/fashion-mnist-color-shift.git
cd fashion-mnist-color-shift
```

从 [Zalando 的数据目录](https://github.com/zalandoresearch/fashion-mnist/tree/master/data/fashion) 下载以下四个文件，原样放到 `data/fashion/`，保留 `.gz` 压缩格式。

Download these four files from [Zalando's data directory](https://github.com/zalandoresearch/fashion-mnist/tree/master/data/fashion), place them in `data/fashion/`, and keep them compressed as `.gz` files.

```text
data/fashion/
  train-images-idx3-ubyte.gz
  train-labels-idx1-ubyte.gz
  t10k-images-idx3-ubyte.gz
  t10k-labels-idx1-ubyte.gz
```

安装依赖并先运行较短的试跑：

Install the dependencies and start with the shorter smoke run:

```sh
python -m pip install -r experiment/requirements.txt
python experiment/main.py --smoke
```

试跑将每个 CNN 的训练设为 1 轮、随机森林设为 30 棵树。运行结束后，打开 [结果目录](experiment/results/) 和 [图表目录](experiment/figures/)。

The smoke run uses one training epoch per CNN and 30 trees for the random forest. When it finishes, open the [results](experiment/results/) and [figures](experiment/figures/) directories.

## 完整运行与参数调整<br>Full runs and options

默认完整运行使用 8 轮 CNN 训练和 200 棵树：

A full run uses 8 CNN epochs and 200 random-forest trees by default:

```sh
python experiment/main.py
```

用参数调整训练轮数、树的数量和训练随机种子；例如：

Adjust the epochs, number of trees, and training seed with command-line options, for example:

```sh
python experiment/main.py --epochs 12 --rf-estimators 300 --seed 1
```

## 去哪里看结果<br>Where to find the results

| 文件或目录<br>File or directory | 内容<br>Contents |
|---|---|
| [summary.json](experiment/results/summary.json) | 三种方法的训练、黑白测试与换色测试准确率<br>Training, black-and-white test, and recolored test accuracy for all three methods |
| [best_classification_report.txt](experiment/results/best_classification_report.txt) | 换色测试中最佳方法的逐类报告<br>Per-class report for the best method on the recolored test |
| [summary.png](experiment/figures/summary.png) | 准确率对照图<br>Accuracy comparison chart |
| [figures/](experiment/figures/) | 混淆矩阵、错分图片、训练曲线和 t-SNE 图<br>Confusion matrices, errors, training curves, and t-SNE plots |

## 本实验做了什么<br>What this experiment adds

保留服装轮廓和类别，只改变前景与背景颜色，构造便于观察的分布变化。两种 CNN 使用同一网络结构，对比是否在训练时随机换色；随机森林提供另一条基线。数据处理、训练、评价和图表生成集中在一个入口，方便复跑和比较。

The experiment keeps silhouettes and labels fixed while changing foreground and background colors. Both CNNs use the same architecture, making recoloring during training the comparison of interest; a random forest supplies another baseline. A single entry point handles data preparation, training, evaluation, and plots for easier reruns and comparison.

[黑白测试样例](experiment/figures/samples_test_id.png) · [换色测试样例](experiment/figures/samples_test_ood.png)

[Black-and-white test samples](experiment/figures/samples_test_id.png) · [Recolored test samples](experiment/figures/samples_test_ood.png)

仓库中保存的一次完整运行结果如下，使用 8 轮训练、200 棵树，两套测试各有 10,000 张图：

The stored results below come from one full run with 8 training epochs, 200 trees, and 10,000 images in each test:

| 方法<br>Method | 黑白测试<br>Black-and-white test | 换色测试<br>Recolored test |
|---|---:|---:|
| 随机森林<br>Random forest | 83.63% | 27.59% |
| CNN | 86.88% | 16.10% |
| CNN + 随机换色<br>CNN + random recoloring | 82.19% | 84.09% |

这次运行中，随机换色训练使 CNN 的换色测试准确率提高 67.99 个百分点，黑白测试准确率下降 4.69 个百分点。

In this run, recoloring during training increased the CNN's recolored-test accuracy by 67.99 percentage points and reduced black-and-white accuracy by 4.69 points.

## 数据与开源基础<br>Data and open-source foundations

数据来自 [Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist)，数据集论文为 Han Xiao、Kashif Rasul 和 Roland Vollgraf 的 [Fashion-MNIST（2017）](https://arxiv.org/abs/1708.07747)。训练使用 PyTorch 与 scikit-learn，具体依赖见 [requirements.txt](experiment/requirements.txt)，数据许可见 [LICENSE](data/fashion/LICENSE)。

The data comes from [Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist), described by Han Xiao, Kashif Rasul, and Roland Vollgraf in the [Fashion-MNIST paper (2017)](https://arxiv.org/abs/1708.07747). Training uses PyTorch and scikit-learn; see [requirements.txt](experiment/requirements.txt) for dependencies and [LICENSE](data/fashion/LICENSE) for the dataset license.

## 注意事项<br>Attention

每次运行会更新 `experiment/results/` 和 `experiment/figures/`，请先保存需要保留的结果。`--smoke` 仍会读取完整数据集并生成图表。表中的准确率对应已保存的那次运行。

Each run updates `experiment/results/` and `experiment/figures/`, so save any outputs you want to keep first. `--smoke` still reads the full dataset and generates plots. The accuracy table describes the stored run.
