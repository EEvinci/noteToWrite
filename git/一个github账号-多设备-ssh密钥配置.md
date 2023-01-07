# 一个github账号-多设备-ssh密钥配置

一个GitHub账号, 可以配置多个ssh密钥, 也就是说一个GitHub账号可以同时关联多个设备

因为单独一个设备访问GitHub账号需要使用ssh或者GPT密钥进行验证

通常使用的是ssh密钥, ssh密钥是对称加密, 分为公钥和私钥, 在生成ssh密钥之后, 将公钥放到GitHub上面, 私钥就是访问github的特定设备保存, 以此来进行特定设备对特定GitHub账号的安全访问

每个设备生成的公钥和私钥都是不一样的, 私钥就是在本地, 公钥放到GitHub上, 然后使用`git -T git@github.com`进行连接



## 参考文档

[多个SSH密钥并存且连接到Github](https://kangzhiheng.top/post/11-more-ssh-in-one-laptop/)

[Git 多台电脑共用SSH Key](https://blog.csdn.net/u012408797/article/details/116196831)

[如何做到一台设备上ssh登录github和gitlab？](https://juejin.cn/post/7109791281890459655)

[设置GitHub SSH密钥](https://blog.csdn.net/ahahayaa/article/details/126561691?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126561691%22%2C%22source%22%3A%22ahahayaa%22%7D)

[不同设备登录github ssh-google search](https://www.google.com/search?q=%E4%B8%8D%E5%90%8C%E8%AE%BE%E5%A4%87%E7%99%BB%E5%BD%95github+ssh&sxsrf=ALiCzsYgnY6t2Bcn13xSnnlmlcqiNP03zg%3A1672398163109&ei=U8WuY4KhBpiI4-EP3MuHiAg&ved=0ahUKEwjClr3TmKH8AhUYxDgGHdzlAYEQ4dUDCA8&uact=5&oq=%E4%B8%8D%E5%90%8C%E8%AE%BE%E5%A4%87%E7%99%BB%E5%BD%95github+ssh&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzIECCMQJzoKCAAQRxDWBBCwA0oECEEYAEoECEYYAFDkA1jQJWDiL2gBcAF4AIABqQGIAagGkgEDMC41mAEAoAEByAEFwAEB&sclient=gws-wiz-serp)

[GItCode的ssh说明](https://gitcode.net/gitcode/help-docs/-/wikis/docs/ssh#rsa-ssh-keys)

