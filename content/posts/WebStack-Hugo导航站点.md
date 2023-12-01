---
title: Hello World
date: 2022-09-29T17:06:11+08:00
tags:
- hugo
- WebStack
categories:
- Tutorial
code:
  maxShownLines: 11
# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

## 安装部署
首先 fork [NavBioIT](https://github.com/shenweiyan/NavBioIT.git) 项目

   ```powershell
   #假设站点文件保存在 D:\sites 下
   D:
   cd D:\sites
   # 使用你自己的仓库地址替换
   git clone --recursive https://github.com/<your_name>/<your_repo_name>.git
   ```

   ```powershell
   cd <your_repo_name>
   #使用主题中包含的站点实例数据
   #/E：复制目录和子目录，包括空的。 /H：复制隐藏和系统文件。 /Y：覆盖现有文件而不提示。
   xcopy /E /H /Y themes\WebStack-Hugo\exampleSite\* .
   ```

## 数据更新

- 将 YAML 格式的站点数据转换为 Excel 表格
```python

#pip install pyyaml pandas openpyxl
import yaml
import pandas as pd

def read_yaml(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        return yaml.safe_load(file)

def yaml_to_dataframe(yaml_data):
    rows = []
    id_counter = 1  # 初始化 id 计数器
    for category in yaml_data:
        taxonomy = category['taxonomy']
        icon = category['icon']
        for term_group in category.get('list', []):
            term = term_group['term']
            for link in term_group['links']:
                title = link.get('title', '')
                description = link.get('description', '')
                url = link.get('url', '')
                logo = link.get('logo', '')
                rows.append([id_counter, 1, taxonomy, term, title, description, url, logo, icon])
                id_counter += 1  # 递增 id
    return pd.DataFrame(rows, columns=['id', 'is_use', 'taxonomy', 'term', 'title', 'description', 'url', 'logo', 'icon'])

# YAML 文件路径
yaml_file_path = './../data/webstack.yml'

# 读取 YAML 文件
yaml_data = read_yaml(yaml_file_path)

# 转换为 DataFrame
df = yaml_to_dataframe(yaml_data)

# 输出到 Excel 文件
excel_file_path = './sites.xlsx'
df.to_excel(excel_file_path, index=False)

```
- Excel表格编辑完成后生成YAML数据
```python
import pandas as pd
import yaml

# 读取数据并筛选
df = pd.read_excel("sites.xlsx")
df = df[df["is_use"] == 1]

# 构建 taxonomy 和 icon 的映射
taxonomy_icon_dict = df.drop_duplicates('taxonomy').set_index('taxonomy')['icon'].to_dict()

# 构建一级菜单
all_list = [{'taxonomy': taxonomy, 'icon': icon, 'list': []} 
            for taxonomy, icon in taxonomy_icon_dict.items()]

# 构建二级菜单项
for term, group in df.groupby('term'):
    links = group[['title', 'logo', 'url', 'description']].to_dict(orient='records')
    term_item = {'term': term, 'links': links}
    
    # 找到对应的一级菜单并添加
    taxonomy = group['taxonomy'].iloc[0]
    for item in all_list:
        if item['taxonomy'] == taxonomy:
            item['list'].append(term_item)
            break

# 输出到 YAML
with open("webstack.yml", 'w', encoding="utf8") as f:
    yaml.dump(all_list, f, allow_unicode=True, default_flow_style=False, sort_keys=False)


```
    
- 爬取站点favicon图标 get_favicons.py
```python
#pip install pandas openpyxl requests pillow
import os
import shutil
import requests
from PIL import Image
from io import BytesIO
from urllib.parse import urlparse
import pandas as pd

def download_and_save_icon(url, filename, proxies=None):
    try:
        result = urlparse(url)
        ico_url = result.scheme + "://" + result.netloc + "/favicon.ico"
        response = requests.get(ico_url, proxies=proxies)
        response.raise_for_status()
        image = Image.open(BytesIO(response.content))
        image.save(filename)
        return True
    except Exception as e:
        print(f"Error downloading {url}: {e}")
        return False

# 读取 Excel 文件
excel_file_path = 'path/to/your/excel/file.xlsx'  # 替换为您的 Excel 文件路径
df = pd.read_excel(excel_file_path)

# 创建 SiteLogo 目录（如果不存在）
if not os.path.exists('SiteLogo'):
    os.makedirs('SiteLogo')

for i, row in df.iterrows():
    title = row['title'].replace(' ', '-')  # 将空格替换为短横线
    url = row['url']
    logo_path = f'SiteLogo/{title}.ico'
    if not os.path.exists(logo_path):
        success = download_and_save_icon(url, logo_path)
        if not success:
            # 使用默认图标替换失败的下载
            shutil.copy('SiteLogo/default.ico', logo_path)

```

