# WebStack-Hugo搭建导航站点


## 安装部署
首先 fork [NavBioIT](https://github.com/shenweiyan/NavBioIT.git) 项目

   ```powershell
   #假设站点文件保存在 D:\sites 下
   D:
   cd D:\sites
   # 使用你自己的仓库地址替换
   git clone --recursive https://github.com/&lt;your_name&gt;/&lt;your_repo_name&gt;.git
   ```

   ```powershell
   cd &lt;your_repo_name&gt;
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
    with open(file_path, &#39;r&#39;, encoding=&#39;utf-8&#39;) as file:
        return yaml.safe_load(file)

def yaml_to_dataframe(yaml_data):
    rows = []
    id_counter = 1  # 初始化 id 计数器
    for category in yaml_data:
        taxonomy = category[&#39;taxonomy&#39;]
        icon = category[&#39;icon&#39;]
        for term_group in category.get(&#39;list&#39;, []):
            term = term_group[&#39;term&#39;]
            for link in term_group[&#39;links&#39;]:
                title = link.get(&#39;title&#39;, &#39;&#39;)
                description = link.get(&#39;description&#39;, &#39;&#39;)
                url = link.get(&#39;url&#39;, &#39;&#39;)
                logo = link.get(&#39;logo&#39;, &#39;&#39;)
                rows.append([id_counter, 1, taxonomy, term, title, description, url, logo, icon])
                id_counter &#43;= 1  # 递增 id
    return pd.DataFrame(rows, columns=[&#39;id&#39;, &#39;is_use&#39;, &#39;taxonomy&#39;, &#39;term&#39;, &#39;title&#39;, &#39;description&#39;, &#39;url&#39;, &#39;logo&#39;, &#39;icon&#39;])

# YAML 文件路径
yaml_file_path = &#39;./../data/webstack.yml&#39;

# 读取 YAML 文件
yaml_data = read_yaml(yaml_file_path)

# 转换为 DataFrame
df = yaml_to_dataframe(yaml_data)

# 输出到 Excel 文件
excel_file_path = &#39;./sites.xlsx&#39;
df.to_excel(excel_file_path, index=False)

```
- Excel表格编辑完成后生成YAML数据
```python
import pandas as pd
import yaml

# 读取数据并筛选
df = pd.read_excel(&#34;sites.xlsx&#34;)
df = df[df[&#34;is_use&#34;] == 1]

# 构建 taxonomy 和 icon 的映射
taxonomy_icon_dict = df.drop_duplicates(&#39;taxonomy&#39;).set_index(&#39;taxonomy&#39;)[&#39;icon&#39;].to_dict()

# 构建一级菜单
all_list = [{&#39;taxonomy&#39;: taxonomy, &#39;icon&#39;: icon, &#39;list&#39;: []} 
            for taxonomy, icon in taxonomy_icon_dict.items()]

# 构建二级菜单项
for term, group in df.groupby(&#39;term&#39;):
    links = group[[&#39;title&#39;, &#39;logo&#39;, &#39;url&#39;, &#39;description&#39;]].to_dict(orient=&#39;records&#39;)
    term_item = {&#39;term&#39;: term, &#39;links&#39;: links}
    
    # 找到对应的一级菜单并添加
    taxonomy = group[&#39;taxonomy&#39;].iloc[0]
    for item in all_list:
        if item[&#39;taxonomy&#39;] == taxonomy:
            item[&#39;list&#39;].append(term_item)
            break

# 输出到 YAML
with open(&#34;webstack.yml&#34;, &#39;w&#39;, encoding=&#34;utf8&#34;) as f:
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
        ico_url = result.scheme &#43; &#34;://&#34; &#43; result.netloc &#43; &#34;/favicon.ico&#34;
        response = requests.get(ico_url, proxies=proxies)
        response.raise_for_status()
        image = Image.open(BytesIO(response.content))
        image.save(filename)
        return True
    except Exception as e:
        print(f&#34;Error downloading {url}: {e}&#34;)
        return False

# 读取 Excel 文件
excel_file_path = &#39;path/to/your/excel/file.xlsx&#39;  # 替换为您的 Excel 文件路径
df = pd.read_excel(excel_file_path)

# 创建 SiteLogo 目录（如果不存在）
if not os.path.exists(&#39;SiteLogo&#39;):
    os.makedirs(&#39;SiteLogo&#39;)

for i, row in df.iterrows():
    title = row[&#39;title&#39;].replace(&#39; &#39;, &#39;-&#39;)  # 将空格替换为短横线
    url = row[&#39;url&#39;]
    logo_path = f&#39;SiteLogo/{title}.ico&#39;
    if not os.path.exists(logo_path):
        success = download_and_save_icon(url, logo_path)
        if not success:
            # 使用默认图标替换失败的下载
            shutil.copy(&#39;SiteLogo/default.ico&#39;, logo_path)

```



---

> 作者:   
> URL: https://jiyanjiyu.com/posts/webstack-hugo%E5%AF%BC%E8%88%AA%E7%AB%99%E7%82%B9/  

