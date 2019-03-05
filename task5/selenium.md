# Task5
安装selenium并学习。
使用selenium模拟登陆163邮箱。
话不多说，直接上代码：
```
from selenium import webdriver
import time
def login():
    dr = webdriver.Chrome()
    #打开登陆163邮箱的网页
    dr.get('http://mail.163.com/')
    #将浏览器窗口最大化
    dr.maximize_window()
    #休息五分钟等待网页加载完毕    time.sleep(5)
    #找到邮箱账号登录框对应的iframe
    dr.switch_to.frame('x-URS-iframe')
    #找到邮箱账号输入框
    email = dr.find_element_by_name('email')
    #将自己的邮箱地址输入到邮箱账号框中
    email.send_keys('18271671990')
    #找到密码输入框
    password = dr.find_element_by_name('password')
    #输入自己的邮箱密码
    password.send_keys('whm112233')
    #找到登陆按钮
    login_btn = dr.find_element_by_id('dologin')
    #点击登陆按钮
    login_btn.click()
    #等待10秒看是否登陆成功        
    time.sleep(10)

if __name__ == '__main__':       
    login()
```
