# 数据分析总流程
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import warnings
# 1. 固定配置
plt.rcParams["font.sans-serif"] = ["SimHei"]
plt.rcParams["axes.unicode_minus"] = False
warnings.filterwarnings("ignore")

# 2. 读取数据
df = pd.read_csv("可视化练习.csv")

# 3. 数据准备（先分组算均值）
group_data = df.groupby("neighbourhood_group")["availability_365"].mean().reset_index()

# 4. 套柱状图模板
def plot_bar(df, x_col, y_col, title):
    plt.figure(figsize=(8, 5))
    sns.barplot(data=df, x=x_col, y=y_col, palette=["#4ECDC4", "#FF6B6B", "#45B7D1", "#96CEB4"])
    plt.title(title, fontsize=12)
    plt.xlabel(x_col, fontsize=10)
    plt.ylabel(y_col, fontsize=10)
    plt.show()

# 5. 调用函数
plot_bar(group_data, "neighbourhood_group", "availability_365", "北京各行政区房源平均可预订天数")
def plot_scatter(df, x_col, y_col, title):
    plt.figure(figsize=(8, 5))
    sns.scatterplot(data=df, x=x_col, y=y_col, color="#45B7D1", s=80, alpha=0.8)
    # 拟合趋势线
    import numpy as np
    z = np.polyfit(df[x_col], df[y_col], 1)
    p = np.poly1d(z)
    plt.plot(df[x_col], p(df[x_col]), color="#FF6B6B", linestyle="--")
    plt.title(title, fontsize=12)
    plt.xlabel(x_col, fontsize=10)
    plt.ylabel(y_col, fontsize=10)
    plt.show()

# 调用函数
plot_scatter(df, "availability_365", "price", "房源可预订天数与价格的相关性")
