# 实战大项目
实战大项目：模拟登录丁香园，并抓取论坛页面所有的人员基本信息与回复帖子内容。

今天的作业没时间做了，准备面试，放一下优秀同学的代码，改天看。

代码：
```
from selenium import webdriver
import time
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, NoSuchElementException

# 声明浏览器对象
browser = webdriver.Chrome()
browser.get("http://www.dxy.cn/bbs/thread/626626#626626")

def login_zhihu(browser):
    try:
        #点击登录
        browser.find_element_by_xpath('//div[@class="nav_account"]/a[1]').click()
        #点击返回电脑登录
        browser.find_element_by_xpath('//div[@class="login__tab_wp"]/a[2]/i').click()
        elem = browser.find_element_by_name("username")
        elem.clear()  # 清空
        elem.send_keys("*****")  # 填入你的账号
        #获取登录密码
        elem = browser.find_element_by_name("password")
        elem.clear()
        elem.send_keys("********") #填上你的密码

        print("开始登陆...")
        browser.find_element_by_xpath("//button").click() #点击登录按钮登录

    except TimeoutException:
        print("Time Out")
    except NoSuchElementException:
        print("No Element")
        

def get_information(browser):
    print("登录成功")
    time.sleep(10)
    print("开始获取信息。。。")
    elems = browser.find_elements_by_css_selector(".auth")  #发帖人姓名
#     conts = browser.find_elements_by_css_selector(".con")  发帖的信息
    for elem in elems:
        auth = elem.find_element_by_tag_name("a")
        print(auth.text)
#     for con in conts:
#         content = con.find_element_by_tag_name("td")
#         print(content.text)
            
# 滚动加载
# def scroll_load(browser):
#     #利用 execute_script() 方法将进度条下拉到最底部
#     browser.execute_script("window.scrollTo(0, document.body.scrollHeight);")
#     browser.implicitly_wait(2)  # 隐式等待
    
# 主函数
def main():
    login_zhihu(browser)  # 登录函数
    #for i in range(2):  #定义滚动次数
    get_information(browser)  # 获取标题与链接
    #scroll_load(browser)  # 滚动
    time.sleep(1)  # 休眠


# 函数入口调用
if __name__ == '__main__':
    main()

    input("按任意键退出-> ")
    browser.quit()


```
## 参考
[参考](https://blog.csdn.net/TNTZS666/article/details/88319980)
