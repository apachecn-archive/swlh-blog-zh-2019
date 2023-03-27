# 忘记受控/不受控组件！

> 原文：<https://medium.com/swlh/forget-about-controlled-uncontrolled-components-e9d053218ca9>

你不应该强迫你的消费者通过所有这些道具！不公平！

昨天，我发现自己不愿意实现另一个 React 组件——一个接受多个点的特殊滑块。我发现[gatan Renaudeau](https://medium.com/u/5d8dd4630ec4?source=post_page-----e9d053218ca9--------------------------------)的多滑块是满足我需求的最简单、最快的解决方案。

在安装它之前，我对这个库进行了快速的源代码检查。在筛选了几行之后，我停在了这里:

```
export default uncontrollable(MultiSlider, {values: 'onChange'});
```