---
{"dg-publish":true,"permalink":"/C-永久笔记/10个Python脚本,轻松实现日常任务自动化/"}
---

本文将向初学者介绍10个实用的Python脚本,帮助你轻松实现日常任务的自动化。这些脚本不仅能节省时间,还能让你更深入地了解Python的强大功能。

## 1\. 自动整理下载文件夹

下载文件夹常常杂乱无章,这个脚本可以根据文件类型自动整理文件:

```python
import os
import shutil

def organize_downloads(path):
    for filename in os.listdir(path):
        name, extension = os.path.splitext(filename)
        extension = extension[1:]
        
        if os.path.exists(path + '/' + extension):
            shutil.move(path + '/' + filename, path + '/' + extension + '/' + filename)
        else:
            os.makedirs(path + '/' + extension)
            shutil.move(path + '/' + filename, path + '/' + extension + '/' + filename)

organize_downloads(r'C:\Users\YourUsername\Downloads')
```

这个脚本会遍历下载文件夹中的所有文件,根据文件扩展名创建相应的子文件夹,并将文件移动到对应的子文件夹中。

## 2\. 批量重命名文件

在处理大量文件时,批量重命名是一项常见需求:

```python
import os

def batch_rename(directory, old_ext, new_ext):
    for filename in os.listdir(directory):
        if filename.endswith(old_ext):
            name_without_ext = os.path.splitext(filename)[0]
            os.rename(
                os.path.join(directory, filename),
                os.path.join(directory, f"{name_without_ext}{new_ext}")
            )

batch_rename(r'C:\Users\YourUsername\Documents', '.txt', '.md')
```

这个脚本可以将指定目录下所有特定扩展名的文件批量<a id="mdh-401a4e"></a>modify

为新的扩展名。

## 3\. 自动备份重要文件

定期备份重要文件是个好习惯,这个脚本可以帮你自动完成:

```python
import shutil
import datetime
import os

def backup_files(source, destination):
    today = datetime.datetime.now().strftime("%Y%m%d")
    dest_dir = os.path.join(destination, f"backup_{today}")
    
    try:
        shutil.copytree(source, dest_dir)
        print(f"Backup completed successfully to {dest_dir}")
    except FileExistsError:
        print(f"Backup for {today} already exists")

backup_files(r'C:\ImportantFiles', r'D:\Backups')
```

这个脚本会在指定的备份目录中创建一个以当前日期命名的文件夹,并将源目录中的所有文件复制到这个新建的备份文件夹中。

## 4\. 监控网站可用性

对于需要保持高可用性的网站,定期检查其状态是非常必要的:

```python
import requests
import time
import smtplib
from email.mime.text import MIMEText

def check_website(url, check_interval=60):
    while True:
        try:
            response = requests.get(url)
            if response.status_code == 200:
                print(f"{url} is up!")
            else:
                send_alert(f"{url} returned status code {response.status_code}")
        except requests.RequestException:
            send_alert(f"Failed to connect to {url}")
        
        time.sleep(check_interval)

def send_alert(message):
    sender = "your_email@example.com"
    receiver = "admin@example.com"
    password = "your_password"
    
    msg = MIMEText(message)
    msg['Subject'] = "Website Alert"
    msg['From'] = sender
    msg['To'] = receiver
    
    with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
        server.login(sender, password)
        server.sendmail(sender, receiver, msg.as_string())

check_website("https://www.example.com")
```

这个脚本会定期检查指定网站的可用性,如果发现问题,就会发送邮件提醒。注意要替换成你自己的邮箱和密码。

## 5\. 自动生成项目报告

在软件开发中,经常需要生成项目报告。这个脚本可以自动收集Git仓库的信息并生成简单的报告:

```python
import git
import datetime
from jinja2 import Template

def generate_report(repo_path):
    repo = git.Repo(repo_path)
    commits = list(repo.iter_commits('master', max_count=10))
    
    template = Template("""
    # Project Report
    
    Generated on: {{ date }}
    
    ## Recent Commits
    
    {% for commit in commits %}
    - {{ commit.hexsha[:7] }}: {{ commit.summary }} ({{ commit.author }})
    {% endfor %}
    """)
    
    report = template.render(
        date=datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        commits=commits
    )
    
    with open('project_report.md', 'w') as f:
        f.write(report)

generate_report('/path/to/your/repo')
```

这个脚本使用GitPython库来获取仓库信息,并使用Jinja2模板引擎生成Markdown格式的报告。

## 6\. 自动化Excel数据处理

处理Excel文件是许多人的日常工作,Python可以大大简化这个过程:

```python
import pandas as pd
import matplotlib.pyplot as plt

def process_excel(input_file, output_file):
    # 读取Excel文件
    df = pd.read_excel(input_file)
    
    # 数据处理
    df['Total'] = df['Quantity'] * df['Price']
    summary = df.groupby('Category')['Total'].sum().sort_values(descending=True)
    
    # 生成图表
    plt.figure(figsize=(10, 6))
    summary.plot(kind='bar')
    plt.title('Sales by Category')
    plt.xlabel('Category')
    plt.ylabel('Total Sales')
    plt.tight_layout()
    plt.savefig('sales_chart.png')
    
    # 写入新的Excel文件
    with pd.ExcelWriter(output_file) as writer:
        df.to_excel(writer, sheet_name='Raw Data', index=False)
        summary.to_excel(writer, sheet_name='Summary')

process_excel('sales_data.xlsx', 'sales_report.xlsx')
```

这个脚本读取销售数据,计算总额,生成汇总统计和图表,然后将结果保存到新的Excel文件中。

## 7\. 自动化PDF处理

PDF文件的处理也是一个常见需求,这里我们来看看如何提取PDF中的文本:

```python
import PyPDF2
import re

def extract_text_from_pdf(pdf_path):
    with open(pdf_path, 'rb') as file:
        reader = PyPDF2.PdfReader(file)
        text = ""
        for page in reader.pages:
            text += page.extract_text() + "\n"
    return text

def find_emails(text):
    email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
    return re.findall(email_pattern, text)

pdf_text = extract_text_from_pdf('document.pdf')
emails = find_emails(pdf_text)
print(f"Found {len(emails)} email addresses:")
for email in emails:
    print(email)
```

这个脚本使用PyPDF2库提取PDF文本,然后使用正则表达式查找其中的邮箱地址。

## 8\. 自动化图片处理

批量处理图片是另一个可以通过Python轻松实现的任务:

```python
from PIL import Image
import os

def batch_resize_images(input_folder, output_folder, size):
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)
    
    for filename in os.listdir(input_folder):
        if filename.endswith(('.png', '.jpg', '.jpeg')):
            with Image.open(os.path.join(input_folder, filename)) as img:
                img.thumbnail(size)
                img.save(os.path.join(output_folder, filename))

batch_resize_images('original_images', 'resized_images', (300, 300))
```

这个脚本使用Pillow库来批量调整图片大小,同时保持原始宽高比。

## 9\. 自动化网络爬虫

网络爬虫可以帮助我们自动收集网络上的信息:

```python
import requests
from bs4 import BeautifulSoup

def scrape_quotes():
    url = "http://quotes.toscrape.com"
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    quotes = []
    for quote in soup.find_all('span', class_='text'):
        quotes.append(quote.text)
    
    return quotes

quotes = scrape_quotes()
for i, quote in enumerate(quotes, 1):
    print(f"{i}. {quote}")
```

这个脚本使用requests库获取网页内容,然后用BeautifulSoup解析HTML并提取引用文本。

## 10\. 自动化日志分析

分析日志文件可以帮助我们了解系统的运行状况:

```python
import re
from collections import Counter

def analyze_log(log_file):
    ip_pattern = r'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}'
    with open(log_file, 'r') as f:
        log_content = f.read()
    
    ip_addresses = re.findall(ip_pattern, log_content)
    ip_counts = Counter(ip_addresses)
    
    print("Top 5 IP addresses:")
    for ip, count in ip_counts.most_common(5):
        print(f"{ip}: {count} times")

analyze_log('server.log')
```

这个脚本使用正则表达式从日志文件中提取IP地址,然后统计出现次数最多的IP地址。

## 结语

这10个Python脚本展示了Python在自动化日常任务方面的强大能力。作为一名经验丰富的Python开发者,我建议你不要止步于此,而是根据自己的实际需求,不断探索和创造新的自动化脚本。记住,编程的乐趣在于解决实际问题,而Python正是一个能让这个过程变得既高效又有趣的工具。

希望这篇文章能够激发你的创意,帮助你在日常工作中更好地运用Python。如果你有任何问题或想法,欢迎与我分享!
