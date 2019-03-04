# xpath学习
## Target
- 学习xpath，使用lxml+xpath提取内容。
- 使用xpath提取丁香园论坛的回复内容。
- 丁香园直通点：[晕厥待查——请教各位同仁 - 心血管专业讨论版 -丁香园论坛](http://www.dxy.cn/bbs/thread/626626#626626) 。
## 代码
```
from lxml import etree
import requests

def get_html(url, headers):
    response = requests.get(url, headers = headers)
    try:
        if response.status_code == 200:
            response.encoding = response.apparent_encoding
            return response.text
    except:
        pass

def  get_parse(html):
    tree = etree.HTML(html)
    user = tree.xpath('//div[@class="auth"]/a/text()')
    replys=[]
    for i in range(1,5):
        p="post_"+str(i)
        reply_info = tree.xpath('//div[@id="'+p+'"]//td[@class="postbody"]/text()')
        reply=''
        for k in reply_info:
            reply+=k.strip()  
        replys.append(reply)

    for users, reply in zip(user, replys):
        print('用户名:'+users, '回复内容：'+ reply.strip())


url = 'http://www.dxy.cn/bbs/thread/626626'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.119 Safari/537.36'
            }
html = get_html(url, headers)
get_parse(html)

```
## 运行结果
```
用户名:楼医生 回复内容：我遇到一个“怪”病人，向大家请教。她，42岁。反复惊吓后晕厥30余年。每次受响声惊吓后发生跌倒，短暂意识丧失。无逆行性遗忘，无抽搐，无口吐白沫，无大小便失禁。多次跌倒致外伤。婴儿时有惊厥史。入院查体无殊。ECG、24小时动态心电图无殊；头颅MRI示小软化灶；脑电图无殊。入院后有数次类似发作。请问该患者该做何诊断，还需做什么检查，治疗方案怎样？
用户名:lion000 回复内容：从发作的症状上比较符合血管迷走神经性晕厥，直立倾斜试验能协助诊断。在行直立倾斜实验前应该做常规的体格检查、ECG、UCG、holter和X-ray胸片除外器质性心脏病。贴一篇“口服氨酰心安和依那普利治疗血管迷走性晕厥的疗效观察”作者：林文华 任自文 丁燕生
用户名:xghrh 回复内容：同意lion000版主的观点：如果此患者随着年龄的增长，其发作频率逐渐减少且更加支持，不知此患者有无这一特点。入院后的HOLTER及血压监测对此患者只能是一种安慰性的检查，因在这些检查过程中患者发病的机会不是太大，当然不排除正好发作的情况。对此患者应常规作直立倾斜试验，如果没有诱发出，再考虑有无可能是其他原因所致的意识障碍，如室性心动过速等，但这需要电生理尤其是心腔内电生理的检查，毕竟是有一种创伤性方法。因在外地，下面一篇文章可能对您有助，请您自己查找一下。心理应激事件诱发血管迷走性晕厥1例 ，杨峻青、吴沃栋、张瑞云，中国神经精神疾病杂志， 2002 Vol.28 No.2
用户名:keys 回复内容：该例不排除精神因素导致的，因为每次均在受惊吓后出现。当然，在作出此诊断前，应完善相关检查，如头颅MIR(MRA),直立倾斜试验等。

```

