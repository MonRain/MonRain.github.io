---
title: Python异常检测开源工具包
categories: Python
tags:
- Python
- 日积月累
---

# 异常检测的工具包PyOD

[GitHub项目地址](https://github.com/yzhao062/pyod)
[文档地址](https://pyod.readthedocs.io/en/latest/)

`PyOD`是在`sklearn`基础上开发的异常检测工具包，[涉及到的检测方法](https://pyod.readthedocs.io/en/latest/#implemented-algorithms)分为三类：

1. Individual Detection Algorithms
2. Outlier Ensembles & Outlier Detector Combination Frameworks
3. Utility Functions

美中不足在于所涉及的所有算法均是`offline`模式，无法满足增量学习（在线学习）的测试需求。（尽管部分模型中存在`partial_fit()`方法）

# 增量学习（在线学习）的异常检测工具包PySAD

[GitHub项目地址](https://github.com/selimfirat/pysad)
[文档地址](https://pysad.readthedocs.io/en/latest/)

`PySAD`也是在`sklearn`基础上开发的异常检测工具包，同时也向`PyOD`兼容，[提供了与PyOD协作的方法](https://pysad.readthedocs.io/en/latest/examples.html#example-pyod-integration)。

[涉及到的检测方法](https://pysad.readthedocs.io/en/latest/api.html#api-reference)可以从官方的API文档中查看。

# 兼顾离线学习于在线学习的检测工具包alibi-detect

[GitHub项目地址](https://github.com/SeldonIO/alibi-detect)
[文档地址](https://docs.seldon.io/projects/alibi-detect/en/latest/)

并未经过实际测试应用，了解不多。

需要更多详情可以参考项目仓库和文档。

