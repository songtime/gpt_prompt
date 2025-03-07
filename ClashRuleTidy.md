
我们进行约定，后续提示词中，我使用Markdown语法<!-- -->的部分，是我自己的注释，你需要忽略解读我的注释。

<!-- 
优化clash规则
-->

我希望对这份yaml文件的 rules字段里面的子键进行重新排序，比如

``` 
    - DOMAIN-SUFFIX, marketplace.visualstudio.com, DIRECT
    - DOMAIN-SUFFIX, vscode.gallery.vsassets.io, DIRECT
    - 'RULE-SET,gemini,🤖openAI'
    - 'DOMAIN-SUFFIX,binance.com,🌍 虚拟币交易'
    - 'DOMAIN-SUFFIX,bnbstatic.com,🌍 虚拟币交易'
``` 
    
    是5个子键。排序规则如下：

1. 分流规则解读：按照每个子建的最后一个元素意味着分流规则，例如：对`'DOMAIN-SUFFIX,bnbstatic.com,🌍 虚拟币交易'`【分流类型】是 🌍 虚拟币交易 ，这个分流规则是指，当域名是binance.com或者是bnbstatic.com的时候，将流量导向🌍 虚拟币交易。子键倒数第二个字段是【分流域名】。

2. 排序法则：按照文件'order.txt'里面的顺序，称为【新顺序】

    2.1 【分流类型】确认，提取yaml的所有分流规则，并去重复，得到一个列表，称为【旧规则】，如果【旧规则】的出现任意一个元素不在【新顺序】里，则给用户提示，程序终止
    2.2 对yaml文件的rules字段进行排序，按照【新顺序】进行排序。

    2.3 同名分流类型，按照【分流域名】的首字母不区分大小写，a-z进行排序。

编写一个python脚本实现上述功能，修改原yaml文件的rules字段，并输出到文件。