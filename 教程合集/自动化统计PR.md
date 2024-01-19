# Python统计PR及Issue数量

本教程用于指导自动化统计 Github 上提交 PR 相关内容。

## 前提条件

- 编译器 (VS Code、Spyder、Pycharm……)，且安装 requests 和 pandas 库。可通过如下命令在终端分别安装。
    1. `pip install requests`
    2. `pip install pandas`

- 申请token

    1. 登录到 GitHub，点击右上角的你的头像，然后选择 __Settings__ 。

    2. 在左侧菜单中，下拉到底部，点击 __Developer settings__ 。

    3. 点击 __Personal access tokens__ -> __Personal Access Tokens__ -> __Tokens (classic)__ -> __Create new token (Classic)__ 。

    4. Note 字段中，输入一个描述（比如 __token for counting PRs__ ）。

    5. Expiration 部分自行选择。

    6. Select scopes 部分，选择 __repo__ 。

    7. 点击页面底部的 __Generate token__ 。

    8. 复制token令牌（务必复制保存）。

## Python统计PR数量

编译器内运行如下代码

```python=
import requests
import pandas as pd
from datetime import datetime
from concurrent.futures import ThreadPoolExecutor

# 你的GitHub令牌
token = "ghp_HcDH60hsjjh92SpPtDsU3Nf1higwTh1nTXL0"

# 你要查询的仓库名和所有者名
# 如需查询其他仓库，更改此处即可
repo = "DaoCloud/DaoCloud-docs"

# 指定日期范围
start_date = "2023-01-01T00:00:00Z"
end_date = "2023-12-31T23:59:59Z"

# GitHub的API endpoint
url = f"https://api.github.com/repos/{repo}/pulls"

headers = {
    "Authorization": f"token {token}",
    "Accept": "application/vnd.github.v3+json",
}

params = {
    "state": "all",  # 获取所有的PR
    "sort": "created",
    "direction": "desc",  # 从新到旧排序
    "per_page": 100,  # 每页的结果数量
}

# 创建一个DataFrame来存储结果
df = pd.DataFrame(columns=["Date", "Author", "Title", "Labels", "Changed Files", "Additions", "Deletions"])

# 定义一个函数来获取PR的详细信息
def get_pr_details(pr):
    # Get PR details
    pr_url = pr["url"]
    pr_response = requests.get(pr_url, headers=headers)
    pr_data = pr_response.json()
    
    changed_files = pr_data["changed_files"]
    additions = pr_data["additions"]
    deletions = pr_data["deletions"]
    
    # Extract the names of all labels
    labels = [label["name"] for label in pr["labels"]]
    
    # Convert the date to a timezone-naive datetime
    created_at_naive = datetime.strptime(pr["created_at"], "%Y-%m-%dT%H:%M:%SZ")
    
    return {
        "Date": created_at_naive,
        "Author": pr["user"]["login"],
        "Title": pr["title"],
        "Labels": labels,
        "Changed Files": changed_files,
        "Additions": additions,
        "Deletions": deletions
    }

page = 1
#max_workers数值可以适当调高，但是太夸张可能会被GitHub API限制
with ThreadPoolExecutor(max_workers=10) as executor: 
    while True:
        params["page"] = page
        response = requests.get(url, headers=headers, params=params)
        data = response.json()
        if not data:
            break
        futures = [executor.submit(get_pr_details, pr) for pr in data if pr["created_at"] >= start_date and pr["created_at"] <= end_date]
        for future in futures:
            df = df.append(future.result(), ignore_index=True)
        page += 1

# 将日期列转换为日期类型，并将其设置为索引
df["Date"] = pd.to_datetime(df["Date"])
df.set_index("Date", inplace=True)

# 计算每个标签的PR数量
label_counts = df["Labels"].explode().value_counts()

# 按月份统计PR数量
monthly_counts = df.resample('M').count()
monthly_counts = df['Author'].resample('M').count().to_frame(name='PR Count')

# 计算每个用户提交的PR年数量和月数量
yearly_user_counts = df.groupby(df.index.year)['Author'].value_counts()
monthly_user_counts = df.groupby([df.index.year, df.index.month])['Author'].value_counts()

# 将结果写入Excel文件
with pd.ExcelWriter('D:\图片\联想截图\\PR_detail0023.xlsx') as writer: 
#保存路径可自行更改
    df.to_excel(writer, sheet_name='PR Details')
    label_counts.to_excel(writer, sheet_name='Label Counts')
    monthly_counts.to_excel(writer, sheet_name='Monthly PR Counts')
    yearly_user_counts.to_excel(writer, sheet_name='Yearly User Counts')
    monthly_user_counts.to_excel(writer, sheet_name='Monthly User Counts')

print(f"Total PR count for 2023: {len(df)}")
```

需要修改的内容：

- token = "此处粘贴申请的字母token"
- repo = "DaoCloud/DaoCloud-docs"
`此处是文档站的仓库，可替换为自己要查询的库`

- start_date = "2023-01-01T00:00:00Z"
- end_date = "2023-12-31T23:59:59Z"
`查询日期，注意无空格`
- with pd.ExcelWriter('D:\图片\联想截图\\PR_detail0023.xlsx') as writer:
`替换括号内的保存地址`
