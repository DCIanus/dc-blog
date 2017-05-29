---
title: 【无聊向】迅雷影音字幕下载API
urlname: api_for_xunlei_subscene
date: 2017-05-28 21:07:32
tags:
---

    本文API已经用Python封装并上传于github
    最近在学前端相关知识，所以小弟自己也用ELectron封装了一个字幕下载器，使用非常简单，只要把视频文件拖入其中，稍微等一小会就会自动下载好字幕到视频所在文件夹并自动更名。
    代码很丑，也没有直接发布打包好的文件，原因很简单……校园网上行带宽只有1Mbps，尝试上传时多次失败……
    另外写了一个WPF版本，已经发布在下面这个项目中
    项目地址：DCjanus/ThunderSubs

**本文没有任何技术含量**

平时喜欢看美剧和英剧，尤其是《神秘博士》。
<!--more-->
平时最喜欢的播放器是PotPlay配上Zune皮肤，简洁非凡。

PotPlay虽然简洁，但是没有自动字幕在线下载，又懒得到处去找字幕。

后发现迅雷影音，其字幕在线匹配功能非常强大，但由于自己的强迫症，并不想用迅雷影音，宁愿用迅雷影音下载好字幕之后再提取出来给PotPlay用。

很明显，这样很不方便，于是尝试抓包获取迅雷影音的字幕下载API，期间各种麻烦就不说了，反正你们也不在意，直接上结论吧23333。

# 1. 计算视频文件cid值

通过一个非常容易被碰撞但是不需要读取整个文件的hash函数计算出一个名为cid的值，算法如下：

    def cid_hash_file(path: str):
    '''
    计算文件名为cid的hash值，算法来源：https://github.com/iambus/xunlei-lixian
    :param path: 需要计算的本地文件路径
    :return: 所给路径对应文件的cid值
    '''
    h = hashlib.sha1()
    size = os.path.getsize(path)
    with open(path, 'rb') as stream:
        if size < 0xF000:
            h.update(stream.read())
        else:
            h.update(stream.read(0x5000))
            stream.seek(size // 3)
            h.update(stream.read(0x5000))
            stream.seek(size - 0x5000)
            h.update(stream.read(0x5000))
    return h.hexdigest().upper()

以神秘博士一集圣诞特别版为例，生成的cid为`B48912B308A91743BF42273436C4EED68FC3E970`

# 2. 获取字幕信息列表

使用get方法访问`http://sub.xmp.sandai.net:8000/subxl/{cid}.json`

其中cid为视频文件的cid值，如示例视频文件对应的链接为：`http://sub.xmp.sandai.net:8000/subxl/B48912B308A91743BF42273436C4EED68FC3E970.json`

其内容正常情况下是一个JSON对象，其“sublist”属性对应的是一个数组，数组中每一个元素都是一个字幕信息的JSON对象。

格式化其中一个结果如下：

    {
        "scid": "86AE53FC9D5A2E41E5E9CAB7C1A3794A1B7206B9",
        "sname": "神秘博士2011圣诞篇The.Doctor.The.Widow.And.The.Wardrobe.ass",
        "language": "简体",
        "rate": "4",
        "surl": "http://subtitle.v.geilijiasu.com/86/AE/86AE53FC9D5A2E41E5E9CAB7C1A3794A1B7206B9.ass",
        "svote": 545,
        "roffset": 4114797192
    }

其中有意义的数据为：

    sname: 字幕文件的原始文件名 
    language: 字幕语言 
    surl: 字幕下载地址

需要注意的是：这个web api非常不稳定，一般需要重试多次，很好奇迅雷这么大的企业是如何做到这么不稳定的……