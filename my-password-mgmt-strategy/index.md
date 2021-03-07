# 密码管理思路

这是一篇给朋友的密码管理思路分享，我想尽量简单地介绍一些保护自己账号安全的方法，尤其是最近爆出安全隐患的苹果账户（Apple ID）。

<!--more-->


---
## 1. Apple ID能干嘛
虽然很多人都在用iPhone，但是其实大部分用户是没意识到Apple ID的重要性的，尤其是一些IT知识比较少的妹子。

如果别人有了你的Apple ID可以干嘛？简单说吧：

- 可以用照片流功能看到你拍摄的照片
- 可以看到你iPhone同步到iCloud里的便签、提醒事项、日历
- 如果你的iPhone开了iCloud文稿同步，那么几乎所有你iPhone里的东西都会被同步到另外一个有你Apple ID密码的人手上，包括微信、邮件、短信聊天记录。
- 甚至可以伪装成你，用你的身份向别人发送iMessage）

去年有过重磅新闻，黑客盗取了大量好莱坞影星的Apple ID密码，并以此进入她们的iCloud相册，导致这些好莱坞影星的艳照泄露。所以Apple ID保护不当的话，确实可能会造成个人数据的严重泄漏，还是需要重视的。

## 2.漏洞简介以及如何应对
上周末Apple的Xcode Ghost漏洞占据大部分科技媒体头条，这个漏洞已经影响到几乎中国的每一台iOS设备，由于Unity等开发软件也被爆出同样的问题，应该说影响范围已经扩大到所有手机系统了。据([乌云测试](http://drops.wooyun.org/papers/9024?url_type=39&object_type=webpage&pos=1))，这个漏洞威胁很大。不确定Apple ID的密码是否会因为这个漏洞而泄露，但比较保险的方式，是在更新各种APP后，修改自己Apple ID的密码，以及开启两步认证，保护自己的Apple ID。

>如何设置两步验证？
1. 前往“[我的 Apple ID](https://appleid.apple.com/cn/account/home)”。
2. 选择“管理您的 Apple ID”，然后登录。
3. 选择“密码和帐户安全”。
4. 在“两步验证”下，选择“开始设置”，并按照屏幕上的说明操作。

## 3.密码管理思路
所以要谈到本文的重点了，如何管理自己的密码？现代社会，几乎所有还在上网的人，都有一大堆密码。QQ、邮箱、微博、各种论坛、银行……而大部分人，为了简单记忆，无论什么帐户，都是同一个密码，而且往往是用生日或姓名简称组合在一起的弱密码。

这样做的风险不用我再废话了，每天都有网站的用户数据库被黑客窃取，更别说有些无良网站自己就在明文存储用户的密码。如果这样做，那么黑客只要知道你一个密码，就可以登陆你的全部帐户。

如果你觉得黑客离你生活太遥远（其实也不远，只是你不知道而已），那么换个例子吧。可能你为了方便，告诉了某个朋友某个论坛的密码，而他出于好奇，用你的账号+密码登录其他网站（QQ邮箱之类的），你的隐私可能就全部暴露了。

所以最基本的密码管理思路就是：不同的服务使用不同的密码。当然，如果有上百个帐户，不可能为此记忆上百个密码，所以为此我们要有一套科学的方法论。以下是我建议的三种管理模式，循序渐进：

### 3.1 密码分层
为全部帐户设置不同的密码几乎是不可能的，但至少你应该为你的关键帐户设置独立的强密码。这些关键帐户包括但不仅限于：Apple ID，网银密码，支付宝，还有你的邮箱。很多人会忽视邮箱，但其实大部分账户密码都是可以通过注册邮箱找回的，所以邮箱密码应该足够强大。

然后如果你很懒，给剩下的帐户使用同一个密码就行了。但如果你还有最基本的隐私保护意识的话，你至少要把社交网站的密码，改成和普通网站的密码不一样。

### 3.2 进一步加强
如果你仅仅是关键帐户使用一个密码，社交网站一个，普通网站一个，那么是比以前安全多了，但还远远不够。关键帐户用同一个密码是极其不安全的，最起码，Apple ID和邮箱密码不应该一样，否则黑客在有窃取Apple ID后，完全可以猜出你的邮箱以及密码，进而进入邮箱，找到你一大堆网站的注册记录。然后你所有的帐户都沦陷了。

那么怎么为不同的网站设置不同的密码？其实如果有一套逻辑清晰算法，那么既可以便于记忆，又能够足够安全。举一个算法的例子：

>想一个“核心码”，把核心码和登录网站的服务商域名，用横杠连接，就是该网站的密码。比如“核心码”是姓名缩写+身高——"cyj186", 登录的帐户是QQ，服务商的域名就是腾讯“tencent"，那么这个密码就设置好了--"cyj186-tencent".

但是以上这个算法很脆弱，黑客看到QQ的密码里有tencent，就很容易猜到你的新浪密码是"cyj186-sina"。 而且最后得出来的密码也不够安全——它没有大写字母，长度不够，也没有特殊符号。

事实上，如果你试着拿这个算法去生成Apple ID的密码，你会发现这一关就无法通过，因为Apple ID的密码规则就是至少要有一个大写字母，一个小写字母以及一个数字。而且不只是Apple，很多网站现在都有类似的规定。

所以可以动动脑筋，将算法给强化。怎么强化可以自己想，这个过程挺有乐趣。

我举个例子，首先还是利用域名作为变量，这样好记忆。但是不要取全部域名，这样容易被猜出来。

比如只取域名的第一位、第二位和最后一位，继续以tencent为例，得到tet，这时密码已经变成”cyj186-tet“了，大部分人不会一下发现tet和腾讯的关联，也就猜不出来你的微博密码是cyj186-weo。

还不够安全，用域名换算成密码是个很常见的方法，仔细观察可以看到tet和tencent的关联。所以可以再用简单的替换加密法，让字母看起来完全没有规律。

举例而言，把变量的每一个字母往后推一位，就可以让你的密码看起来完全没有规律。(被迫害妄想症患者可以选择第一个字母加一位，第二个字母加三位，第三个字母减一位之类的更无规律算法）


| 变换前| 变换后| 
| --------- |:---------:|
| t| u |
| e | f|
| t | u | 

然后还不够，记得我们需要大写字母和特殊符号吗？改一改核心码就行了，并不会很复杂。比如cyj186，这个核心码太短了，可以改成chenyj186, 将首字母大写，并把h这个字母，用形状类似的#代替。这也是一种加密方法，叫做leet。类似的例子还有把"i"用"!"代替，把“s"用"$"代替之类。[注1]

最后，你得到了"C#enyj186-ufu"这个密码，大部分人即使获取了这个密码，也无法猜到你其它的帐户密码会是什么。

肯定有人会认为这个方法太变态太复杂了，但其实你想想吧，核心码C#en186是一直不变的，你很快就能熟练输入这串数字。只不过尾数那个由域名产生变量是你计算出来的，所以记忆非常简单，临时想不起某个密码时，使用这个算法也很容易推算出来。

事实上，只用域名作为变量还不够安全，我推荐对于真正核心的密码，比如邮箱以及支付宝，应该找一个和域名没啥关系的单词，将这个单词用替换加密法转换后，和核心码结合作为密码。举例而言，我曾经用几个很喜欢的电影女神的名字，Leet后，加上核心码，分别作为支付宝、Gmail和Apple ID的密码，不仅好记，输入密码时还有点小激动呢。

### 3.3 终极管理方案--软件
最终极的解决方案可能是软件，但它未必是最好的。介绍文章可以看[这里](http://www.iplaysoft.com/1password.html)。没用过的人可能比较难理解软件如何管理密码，可以下载个试用版感受下便知。

简单说，这些密码管理软件都是一个思路，就是建立一个加密文件，储藏所有你的密码信息。你打开这个加密文件是需要一个主密码的（为了不被暴力破解，主密码应该足够长并且复杂），所以你只要记住一个主密码，并且确保它不泄露即可。

还要记住永远别丢失这个加密文件，经常备份它，现在主流做法是把这个文件放到Dropbox之类的网盘。别担心网盘服务商打开它，理论上没有主密码，他们是破解不了这个文件的。

在习惯这个软件后，你可以用它为你所有账户生成不同的密码，生成的密码是类似于“OIj1FvZOkk1BaROSs9QH“这样的，所以基本没有暴力破解它的可能性。当然你也基本不可能记住它了，得依赖这个软件帮你自动输入。

当你用主密码打开这个加密文件后，你可以用多种方式输入密码，可以复制粘贴，或者是用浏览器的插件，自动代填密码。后者是否方便，是这个软件你是否能坚持使用下去的关键。现在市面上此类软件很多，但售价不一，主要区别就是它们的自动记忆密码以及自动代填功能是否足够方便。

软件方面，最主流的三款应该是1Password，LastPass和KeePass。我认为最强的解决方案应该是1Password，整体体验非常好，但是缺点是极其昂贵。Windows，OSX和iOS等不同平台的授权都要分开购买，买下一套来得要几百RMB。

KeePass是穷人的选择，完全开源，免费，轻量级，也很好用，但自动填密码的功能和1Password有较大差距。

## 4.我目前的解决方案
我在使用一段时间破解版的1Password后，觉得自己需求不够大，所以并没有选择付费购买1Password。但更重要的原因，是1Password之类的解决方案，毕竟是把所有密码信息放在一个地方。虽然没有证据显示，1Password存在漏洞可以被破解。但这种把鸡蛋放在一个篮子里的做法，可能发生“主密码太久没输入忘记了”/“被一个美女用糖衣炮弹攻势成功骗取到主密码”/“黑客使用社工/键盘记录木马等方式，偷取到你的主密码”这一类惨剧的发生。

所以我选择设计一个好的算法，用密码分层+替换算法的方式，自己记忆。有一些辅助记忆文字信息，我记录到了Onenote的加密分区中。要打开Onenote，需要知道我的微软账户密码，此外笔记本是用不同的密码加密的，双重加密尽量确保安全。此外也使用了KeePass记忆一些信用卡、账户信息作为记忆的辅助。

----
我估计大部分人看完这篇文章后，还是会觉得这种密码管理方案过于复杂。但至少我建议把Apple ID、支付宝这些关键密码和其他网站的密码分开来。而且算法复杂可以自己简化，只要贯彻一个思路，就是尽量地使用不同密码，以及设计一个只有自己才知道的算法。真的尝试这个方法一段时间后，你会发现其实一点都不会不便。

----
[1]. 这种替换方法是很多人的加密方式，好记又有点安全性。常用的Leet表在[这里](https://zh.wikipedia.org/wiki/Leet)有个介绍。



