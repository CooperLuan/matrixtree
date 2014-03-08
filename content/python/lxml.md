Title: lxml/XPath 匹配入门
Date: 2014-02-28 23:00:00
Category: Python
Tags: lxml,XPath,W3C
Author: Cooper
Summary: 在 lxml 中使用 XPath 匹配 HTML

## 用途

lxml 主要用来写方便维护的 html 解析器，用来代替 PyQuery/BeautifulSoup 和杂乱无章的正则

## XPath in w3c

XPath 的标准来自 [W3School XPath 教程](http://www.w3school.com.cn/xpath/index.asp), 下文是 `lxml.xpath` 支持的所有表达式/函数

## XPath in lxml

以 [京东首页](http://jd.com) 的 etree 对象为例

    :::python
    from StringIO import StringIO
    from lxml import etree
    import requests

    url = 'http://jd.com'
    parser = etree.HTMLParser()

    resp = requests.get(url)
    html = resp.decode(resp.encoding)
    tree = etree.parse(StringIO(html), parser)


1. 表达式 `//` 任意位置节点 `//ul`
2. 表达式 `..` 父节点 `//div[@id='_JD_ALLSORT']/div/..`
3. 表达式 `@` 属性 `//div[@id='_JD_ALLSORT']/@class` == `mc`
4. 路径表达式 `//@lang` `//@id` 列出所有元素的 id
5. 第一个元素 `book[1]` `//div[@id='_JD_ALLSORT']/div[1]`
6. 最后一个元素 `book[last()]` `//div[@id='_JD_ALLSORT']/div[last()]`

        :::python
        print tree.xpath("//div[@id='_JD_ALLSORT']/div[last()]")[0].itertext().next()
        # 彩票

7. 位置在序列中小于3的元素 `book[position()<3]`
8. 有 `id` 属性的 `div` 元素 `//div[@id]`
9. 指定元素属性 `//div[@id='food']`
10. 筛选标签内容为数值且在一定范围内的标签 `//book[price>35]` price 标签大于 35 的 book 标签

        :::python
        # 京东首页
        # 价格大于 50 的商品价格集合
        print tree.xpath("//div[@class='p-price'][node()[2]>50]/node()[2]")
        # 价格大于 50 的商品名称集合
        print tree.xpath("//div[@class='p-price'][node()[2]>50]/preceding-sibling::div/a/text()")

11. 选取若干路径 `//title|//footer`

        :::python
        # 标题 + 价格
        result = tree.xpath("//div[@class='p-price'][node()[2]>50]/node()[2]|//div[@class='p-price'][node()[2]>50]/preceding -sibling::div/a/text()")
        result = dict((result[i *2], result[i*2+1]) for i in range(len(result)/2))
        for title, price in result.items():
            print title, price
        # 天然老料！印度小叶紫檀佛珠 288
        # 新品骆驼camkids 男女儿童休闲鞋 129

12. 所有父节点 `ancestor` `//div[@id='food']/ancestor::*`
13. 包括自身的所有父节点 `ancestor-or-self` `//div[@id='food']/ancestor-or-self::*`
14. 所有属性 `attribute` `//div[@id='food']/attribute::*`
15. 子元素 `child`
16. 所有子节点 `node()` 包括非标签的文本节点 `//div[id='food']/node()`
16. 所有后代元素 `descendant`
17. 包括自身的所有后代元素 `descendant-or-self`
18. 节点之后的所有节点 `following` 结果巨多
19. 节点之后的所有同级节点 `following-sibling`
20. 父节点 `parent`
21. 之前的所有节点 `preceding` 结果巨多
22. 节点之前的所有同级节点 `preceding-sibling`
23. 当前节点 `self`
24. 运算符 `+` `//div[@class='p-price'][node()[2]>50]/node()[2]+30`
25. 运算符 `+/-/*/div/=/!=/</<=/>/>=/or/and/mod`
26. 数值函数 `number` `number(//div[@class='p-price'][node()[2]>50]/node()[2])`
27. 数值函数 `number/abs/ceiling/floor/round`
28. 字符串函数 `string/concat/substring/string-length/normalize-space/translate/contains/starts-with/substring-car/substring-after/`

        :::python
        print tree.xpath("normalize-space('   hello     tank')")
        # hello tank

29. boolean 函数 `boolean/not/true/false`
30. 节点函数 `name/local-name`
31. 合计函数 `count/sum` `sum(//div[@class='p-price'][node()[2]>50]/node()[2])`