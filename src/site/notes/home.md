---
{"dg-publish":true,"permalink":"/home/","title":"主页","tags":["gardenEntry"]}
---


<div style="text-align:center;" ><font size="92px">😎</font></div>

hello 👋，我是 十二时镜 ❄️，一个运维实施工程师。欢迎来到我的数字花园。

## 标签云

```chartsview
#-----------------#
#chart type    -#
#-----------------#
type: WordCloud

#-----------------#
#chart data    -#
#-----------------#
data: | 
  dataviewjs: 
  return (() => {
    const tags = this.app.metadataCache.getTags();
   
    let dataArray = [];
    Object.keys(tags).forEach(key => dataArray.push ({tag: key.replace("#",""),count: tags[key]}));
    return dataArray;
   })();

#-----------------#
#chart options -#
#-----------------#
options:
  wordField: "tag"
  weightField: "count"
  colorField: "tag"
  enableSearchInteraction:
    field: "tag"
    operator: "tag"
style:
  backgroundColor: "translucent"

#-----------------------------------------------#
#--- 可选择多彩颜色(colorField) 或单色 (color) ---#
#---  colorField: "tag" ---#
#---  color: "#bb5548" ---#
#-----------------------------------------------#

  wordStyle:
    rotation: 0
```
## 最近创建

- [[1-日记/20240910\|20240910]]
- [[4-工作笔记/BIP合同中止回退SQL\|BIP合同中止回退SQL]]
- [[A-生活规划/Myideas\|Myideas]]
- [[C-永久笔记/10个Python脚本,轻松实现日常任务自动化\|10个Python脚本,轻松实现日常任务自动化]]
- [[C-永久笔记/DevOps人员常用的176条Linux命令速查表\|DevOps人员常用的176条Linux命令速查表]]
- [[C-永久笔记/JAVA基础知识点总结\|JAVA基础知识点总结]]
- [[C-永久笔记/使用cloudflare代理被禁API服务\|使用cloudflare代理被禁API服务]]
- [[C-永久笔记/别人眼中的程序员VS实际中的程序员\|别人眼中的程序员VS实际中的程序员]]
- [[C-永久笔记/各类转发代理服务\|各类转发代理服务]]
- [[C-永久笔记/大脑工作图\|大脑工作图]]

{ .block-language-dataview}

## 最近编辑

- [[home\|home]]
- [[1-日记/20240910\|20240910]]
- [[back\|back]]
- [[C-永久笔记/大脑工作图\|大脑工作图]]
- [[C-永久笔记/各类转发代理服务\|各类转发代理服务]]
- [[A-生活规划/Myideas\|Myideas]]
- [[C-永久笔记/无损音乐下载软件推荐\|无损音乐下载软件推荐]]
- [[C-永久笔记/JAVA基础知识点总结\|JAVA基础知识点总结]]
- [[4-工作笔记/BIP合同中止回退SQL\|BIP合同中止回退SQL]]
- [[C-永久笔记/别人眼中的程序员VS实际中的程序员\|别人眼中的程序员VS实际中的程序员]]

{ .block-language-dataview}
---


<div style="text-align:center;"><font color="#595959">------我是有底线的------</font></div>