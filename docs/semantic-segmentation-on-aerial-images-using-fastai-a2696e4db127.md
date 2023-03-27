# 基于 fastai 的航空影像语义分割

> 原文：<https://medium.com/swlh/semantic-segmentation-on-aerial-images-using-fastai-a2696e4db127>

航空和卫星图像给了我们鸟瞰地球的独特机会。它正被用于[测量森林砍伐](https://www.bu.edu/research/articles/satellite-maps-deforestation/)，[绘制自然灾害后受损区域的地图](https://www.hotosm.org/updates/2017-03-15_imagery_released_for_cyclone_enawo_to_support_mapping_activities)，[发现被洗劫的考古遗址](http://journal.frontiersin.org/article/10.3389/fict.2017.00004/full)，并且有更多当前未开发的用例。由于这些图像的分辨率很高，人眼很难从数据中发现相关信息。这就是计算机视觉为获得洞察力增加卓越价值的地方。

逐像素图像分割在计算机领域是一项具有挑战性和高要求的任务。