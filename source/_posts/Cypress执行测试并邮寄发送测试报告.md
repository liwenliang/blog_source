---
title: Cypress执行测试并邮寄发送测试报告
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 13:36:49
---

前言
为了高质量的软件交付，组织必须转向自动化流程，手动流程太容易出错，效率低下。对于执行CD和devops的团队来说，自动化变得越来越重要，大家只需要专注在code和review中，其它过程都尽可能的自动化。

Jenkins执行python脚本运行测试并邮件发送报告
```bash
"""
 * Create by dell on 2020/9/16
 * Author ：wencheng
 * 微信公众 ：自动化测试 To share

"""

# coding=gbk
import mimetypes
import smtplib
from email import encoders
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email.mime.text import MIMEText
import time
import zipfile
import os
import shutil


def move_file():
    origin_path = r"D:\workspace\test\cypress\screenshots"
    origin_path3 = r"D:\workspace\test\test-report.json"
    new_file_name = r"D:\workspace\test\mochawesome-report"
    shutil.move(origin_path, new_file_name)
    shutil.move(origin_path3, new_file_name)


def npx_bulid():
    command = r"cd D:\workspace\test &&
                npx mochawesome-merge cypress/results/*.json >
                 test-report.json" \
              r"&&npx mochawesome-report-generator 
              test-report.json"
    os.system(command)


def clearfile():
    shutil.rmtree(r"D:\workspace\test\cypress\results")
    shutil.rmtree(r"D:\workspace\test\mochawesome-report")


def file_tozip():
    startdir = "mochawesome-report"  # 要压缩的文件夹路径
    file_news = 'mochawesome-report.zip'  # 压缩后文件夹的名字
    z = zipfile.ZipFile(file_news, 'w', zipfile.ZIP_DEFLATED)  # 参数一：文件夹名
    for dirpath, dirnames, filenames in os.walk(startdir):
        fpath = dirpath.replace(startdir, '')  # 这一句很重要，不replace的话，就从根目录开始复制
        fpath = fpath and fpath + os.sep or ''  # 这句话理解我也点郁闷，实现当前文件夹以及包含的所有文件的压缩
        for filename in filenames:
            z.write(os.path.join(dirpath, filename), 
            fpath + filename)
    print('压缩成功')
    z.close()


def send_report():
    filepath = "mochawesome-report.zip"
    smtp_server = "smtp.exmail.qq.com"
    username = "xxxxxxx "
    password = "xxxxxxx"
    sender = 'xxxxxxx'
    # receivers = ["xxxxxxx","xxxxxxx","xxxxxxx"]  # 接收人
    receivers = ['xxxxxxx']
    current_time = time.strftime('%Y-%m-%d-%H-%M-%S',
                 time.localtime(time.time()))
    msg = MIMEMultipart()
    # 邮件正文
    msg.attach(MIMEText("Hi，All："
                        "\n  test运营系统的自动化脚本测试工具 {}
                        已经完成测试报告见附件"
                        "\n  脚本构建地址：http://xx.xx.xxx.xx:xxxx/"
                        "\n  账号：admin 密码：123456"
                        "\n  目前以实现测试环境xx
                        条数,.".format(current_time), 'plain', 'utf-8'
                        ))
    msg['From'] = sender
    msg['To'] = ";".join(receivers)  # 多个收件人list转str

    subject = "{}的自动化测试报告".format(current_time)
    msg['Subject'] = subject

    data = open(filepath, 'rb')
    ctype, encoding = mimetypes.guess_type(filepath)
    if ctype is None or encoding is not None:
        ctype = 'application/octet-stream'
    maintype, subtype = ctype.split('/', 1)
    file_msg = MIMEBase(maintype, subtype)
    file_msg.set_payload(data.read())
    data.close()
    encoders.encode_base64(file_msg)  # 把附件编码
    file_msg.add_header('Content-Disposition', 'attachment',
     filename="test.zip")  # 修改邮件头
    msg.attach(file_msg)
    try:
        server = smtplib.SMTP(smtp_server, 25)
        server.login(username, password)
        server.sendmail(sender, receivers, msg.as_string())
        server.quit()
        print("发送成功")
    except Exception as err:
        print("发送失败")
        print(err)


if __name__ == '__main__':
    npx_bulid()
    move_file()
    file_tozip()
    send_report()
    clearfile()
```
