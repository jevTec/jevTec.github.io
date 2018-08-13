---
title:"this is a test"
---

### 如何安装GlobalSSH？
* 要使用GlobalSSH需要先下载CLI，[点击这里下载](http://url.com/)（MD5：这里写MD5）。
* （Required）Mac OSX：UCloud CLI整合到Homebrew里，消除用户对UCloud CLI的不信任感。（TODO：直接叫globalssh？根据情况看，先采取标准做法，推UCloud CLI）
* （Optional）Linux
* （Optional）Windows

### 什么GlobalSSH？
* 加速国内用户通过SSH管理海外的服务器，完成26个字母回显速度提高28%（TODO：计算不同丢包比例下的效率提升）

### 使用GlobalSSH

* 核心案例：加速AWS的EC2 （注：一定要是AWS服务器，因为AWS是最通用的）


* 案例2：加速GCE的VM（注：通过GCE来告诉用户，globalssh支持多云，和ucloud没有关系）
* 案例3：加速DigitalOcean的VM（注：再重复一次，共三次，彻底加深印象）
* 案例4：通过CLI，注册UCloud账号，完成购买海外主机、EIP，globalssh加速。（注：这里引入UCloud。但这个例子冗长，要注意控制好篇幅）

### 产品原理
（Required）GlobalSSH的原理？
* 告诉用户GlobalSSH工作于TCP层，是一个TCP Proxy。（为了消除用户提到过的不安全感，用户觉得SSH密码可能泄漏）
* 告诉用户是一个具备高可用的TCP Proxy（show肌肉，因为PathX具备了高可用/容灾能力）

### 产品价格

收费项 | 价格
---|---
加速实例 | 60元/个/月
流量费用 | 2元/GB/日，当日不足1GB免费

<br />如果你对GlobalSSH有任何疑问或建议，请联系[help@globalssh.io](mailto://help@globalssh.io)

<!--
## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/jevTec/jevTec.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/jevTec/jevTec.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

-->
