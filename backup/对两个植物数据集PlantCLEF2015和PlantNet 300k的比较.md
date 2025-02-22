我现在有了两个不同的数据集，我想要比较这两个数据集的数据的重复度。因此编写python程序，首先汇总各个数据集的植物，生成包含文件名的json文件。
首先是对plantnet 300k该植物数据集进行数据统计。
统计的python程序如下：
```python
import os
import json

def collect_species_names():
    """收集当前目录下的所有植物名称并保存为JSON格式"""
    # 获取当前目录
    current_dir = os.path.dirname(os.path.abspath(__file__))
    
    # 收集物种名称
    species_names = []
    for item in os.listdir(current_dir):
        # 只处理文件夹
        if os.path.isdir(os.path.join(current_dir, item)):
            species_names.append(item)
    
    # 按字母顺序排序
    species_names.sort()
    
    # 创建带编号的字典
    species_dict = {str(i): name for i, name in enumerate(species_names)}
    
    # 保存为JSON文件
    output_file = os.path.join(current_dir, 'test_species_names.json')
    with open(output_file, 'w', encoding='utf-8') as f:
        json.dump(species_dict, f, ensure_ascii=False, indent=2)
    
    # 打印统计信息
    print(f"共收集到 {len(species_names)} 个物种名称")
    print(f"数据已保存到: {output_file}")
    
    # 打印前几个示例
    print("\n前5个物种示例:")
    for i in range(min(5, len(species_names))):
        print(f"{i}: {species_names[i]}")

if __name__ == "__main__":
    collect_species_names() 
```

在命令行执行：
```markdown
python collect_species_names.py
```
运行结果：
![Image](https://github.com/user-attachments/assets/ca27eea3-d8f4-4588-9364-74c3cab6bb58)

其次是对plantclef 2015数据集进行数据统计。
运行结果：
![Image](https://github.com/user-attachments/assets/e3df3cbd-e00f-4f4d-9b70-a5a94ca26750)

新建match文件夹。

![Image](https://github.com/user-attachments/assets/59e257b1-40b2-4863-96a8-187aa6461701)
将两个json文件都放入其中。
为了方便鉴别，我将两个文件分别命名如下：

![Image](https://github.com/user-attachments/assets/52223469-cce2-4ebf-985a-cae81e9facf1)

编写对应的Python脚本：
```markdown
import json
from collections import Counter
import pandas as pd
from datetime import datetime

def load_json_data(file_path):
    """加载JSON文件数据，返回物种名称列表"""
    with open(file_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
        # 只返回物种名称（JSON中的值），忽略序号（键）
        return list(data.values())

def analyze_datasets(plantclef_path, plantnet_path):
    """分析两个数据集的异同"""
    # 加载数据
    plantclef_data = load_json_data(plantclef_path)
    plantnet_data = load_json_data(plantnet_path)
    
    # 转换为集合以便比较
    plantclef_species = set(plantclef_data)
    plantnet_species = set(plantnet_data)
    
    # 计算统计信息
    common_species = plantclef_species.intersection(plantnet_species)
    only_in_plantclef = plantclef_species - plantnet_species
    only_in_plantnet = plantnet_species - plantclef_species
    
    # 打印分析结果
    print(f"数据集分析结果:")
    print(f"PlantCLEF2015 物种总数: {len(plantclef_species)}")
    print(f"PlantNet300K 物种总数: {len(plantnet_species)}")
    print(f"两个数据集共有物种数: {len(common_species)}")
    print(f"仅在PlantCLEF2015中出现的物种数: {len(only_in_plantclef)}")
    print(f"仅在PlantNet300K中出现的物种数: {len(only_in_plantnet)}")
    
    # 生成详细的Excel报告
    generate_excel_report(plantclef_species, plantnet_species, 
                         common_species, only_in_plantclef, only_in_plantnet)
    
    # 保存详细结果到文本文件
    with open('analysis_results.txt', 'w', encoding='utf-8') as f:
        f.write("共同物种列表:\n")
        f.write("\n".join(sorted(common_species)))
        f.write("\n\n仅在PlantCLEF2015中的物种:\n")
        f.write("\n".join(sorted(only_in_plantclef)))
        f.write("\n\n仅在PlantNet300K中的物种:\n")
        f.write("\n".join(sorted(only_in_plantnet)))

def generate_excel_report(plantclef_species, plantnet_species, 
                         common_species, only_in_plantclef, only_in_plantnet):
    """生成详细的Excel报告"""
    # 创建时间戳
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    
    # 创建Excel写入器
    with pd.ExcelWriter(f'plant_datasets_analysis_{timestamp}.xlsx') as writer:
        # 1. 概述表格
        summary_data = {
            '统计项': [
                'PlantCLEF2015数据集物种总数',
                'PlantNet300K数据集物种总数',
                '共同物种数量',
                '仅在PlantCLEF2015中的物种数量',
                '仅在PlantNet300K中的物种数量',
                '物种并集总数',
                'PlantCLEF2015独有物种占比',
                'PlantNet300K独有物种占比',
                '共同物种占比(相对于并集)'
            ],
            '数值': [
                len(plantclef_species),
                len(plantnet_species),
                len(common_species),
                len(only_in_plantclef),
                len(only_in_plantnet),
                len(plantclef_species.union(plantnet_species)),
                f"{len(only_in_plantclef)/len(plantclef_species):.2%}",
                f"{len(only_in_plantnet)/len(plantnet_species):.2%}",
                f"{len(common_species)/len(plantclef_species.union(plantnet_species)):.2%}"
            ]
        }
        pd.DataFrame(summary_data).to_excel(writer, sheet_name='总体统计', index=False)
        
        # 2. 共同物种列表
        pd.DataFrame(sorted(common_species), columns=['物种名称']).to_excel(
            writer, sheet_name='共同物种', index=False)
        
        # 3. 仅在PlantCLEF2015中的物种
        pd.DataFrame(sorted(only_in_plantclef), columns=['物种名称']).to_excel(
            writer, sheet_name='PlantCLEF2015独有', index=False)
        
        # 4. 仅在PlantNet300K中的物种
        pd.DataFrame(sorted(only_in_plantnet), columns=['物种名称']).to_excel(
            writer, sheet_name='PlantNet300K独有', index=False)

if __name__ == "__main__":
    plantclef_path = "plantclef2015.json"
    plantnet_path = "plantnet300k.json"
    analyze_datasets(plantclef_path, plantnet_path) 
```
结果如下：
![Image](https://github.com/user-attachments/assets/ca630a50-5f27-4292-9c45-48504dc717d7)

同时生成了一个excel可以看具体情况：

![Image](https://github.com/user-attachments/assets/21fa8d2b-76ec-443f-a31d-b0bf4eb53fe0)

![Image](https://github.com/user-attachments/assets/9292894b-76e7-4704-aed7-240a3e21fbf5)

同时生成html图表展示。生成的代码如下：
```markdown
import json
import os
import plotly.graph_objects as go
from plotly.subplots import make_subplots

def load_json_data(file_path):
    """加载JSON文件数据，返回物种名称列表"""
    with open(file_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
        return list(data.values())

def create_visualizations(plantclef_path, plantnet_path):
    """生成可视化图表并保存为HTML文件"""
    # 加载数据
    plantclef_data = load_json_data(plantclef_path)
    plantnet_data = load_json_data(plantnet_path)
    
    # 转换为集合以便比较
    plantclef_species = set(plantclef_data)
    plantnet_species = set(plantnet_data)
    
    # 计算统计信息
    common_species = plantclef_species.intersection(plantnet_species)
    only_in_plantclef = plantclef_species - plantnet_species
    only_in_plantnet = plantnet_species - plantclef_species
    
    # 创建文件夹
    if not os.path.exists('visualizations'):
        os.makedirs('visualizations')
    
    # 创建子图
    fig = make_subplots(rows=6, cols=1, 
                        subplot_titles=("物种总数对比", "物种分布情况", "物种数量分布", "物种重叠度热力图", "物种数量箱线图", "物种数量散点图"),
                        specs=[[{"type": "bar"}], [{"type": "pie"}], [{"type": "bar"}], [{"type": "heatmap"}], [{"type": "box"}], [{"type": "scatter"}]])

    # 1. 物种总数的柱状图
    fig.add_trace(go.Bar(x=['PlantCLEF2015', 'PlantNet300K'], 
                          y=[len(plantclef_species), len(plantnet_species)],
                          marker_color=['blue', 'orange']),
                  row=1, col=1)

    # 2. 共同物种和独有物种的饼图
    sizes = [len(common_species), len(only_in_plantclef), len(only_in_plantnet)]
    labels = ['共同物种', '仅在PlantCLEF2015中', '仅在PlantNet300K中']
    fig.add_trace(go.Pie(labels=labels, values=sizes, hole=.3),
                  row=2, col=1)

    # 3. 物种数量的条形图
    fig.add_trace(go.Bar(x=['共同物种', '仅在PlantCLEF2015中', '仅在PlantNet300K中'], 
                          y=[len(common_species), len(only_in_plantclef), len(only_in_plantnet)],
                          marker_color=['green', 'red', 'purple']),
                  row=3, col=1)

    # 4. 物种重叠度的热力图
    overlap_matrix = [
        [len(common_species), len(only_in_plantclef), len(only_in_plantnet)],
        [len(only_in_plantclef), len(common_species), len(only_in_plantnet)],
        [len(only_in_plantnet), len(common_species), len(only_in_plantclef)]
    ]
    
    fig.add_trace(go.Heatmap(z=overlap_matrix, 
                           x=['共同物种', '仅在PlantCLEF2015中', '仅在PlantNet300K中'],
                           y=['共同物种', '仅在PlantCLEF2015中', '仅在PlantNet300K中'],
                           colorscale='Viridis'),
                  row=4, col=1)

    # 5. 物种数量的箱线图
    fig.add_trace(go.Box(y=[len(plantclef_species), len(plantnet_species)], 
                          name='物种数量', 
                          marker_color='lightblue'),
                  row=5, col=1)

    # 6. 物种数量的散点图
    fig.add_trace(go.Scatter(x=['PlantCLEF2015', 'PlantNet300K'], 
                              y=[len(plantclef_species), len(plantnet_species)],
                              mode='markers+lines', 
                              marker=dict(size=10, color='red')),
                  row=6, col=1)

    # 更新布局
    fig.update_layout(title_text='植物数据集分析', height=1500)
    
    # 添加meta标签以指定字符集为UTF-8
    fig_html = fig.to_html(full_html=True)
    fig_html = fig_html.replace('<head>', '<head><meta charset="UTF-8">')

    # 保存为HTML文件
    with open('visualizations/species_analysis.html', 'w', encoding='utf-8') as f:
        f.write(fig_html)

if __name__ == "__main__":
    plantclef_path = "plantclef2015.json"
    plantnet_path = "plantnet300k.json"
    create_visualizations(plantclef_path, plantnet_path)

```

图表展示：

![Image](https://github.com/user-attachments/assets/fc066488-a3b7-45da-a62d-1b34277c002d)

![Image](https://github.com/user-attachments/assets/def872de-cc46-4ee7-b7e0-53de7e271784)

![Image](https://github.com/user-attachments/assets/67d292a0-efa2-473b-bfaf-52c366e3a547)

![Image](https://github.com/user-attachments/assets/b5789121-a3f3-496b-8ee2-fb8372b9fe3f)

![Image](https://github.com/user-attachments/assets/4ffdb54c-bbbf-41ba-a021-b263e78f1d10)

![Image](https://github.com/user-attachments/assets/a0626366-d539-4c6f-bfb4-fa3c94fb9a6b)

同时统计总共的物种数量：
```markdown
import json

def load_json_data(file_path):
    """加载JSON文件数据，返回物种名称列表"""
    with open(file_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
        return list(data.values())

def analyze_species(plantclef_path, plantnet_path):
    """分析两个数据集的物种信息并返回统计结果"""
    # 加载数据
    plantclef_data = load_json_data(plantclef_path)
    plantnet_data = load_json_data(plantnet_path)
    
    # 转换为集合以便比较
    plantclef_species = set(plantclef_data)
    plantnet_species = set(plantnet_data)
    
    # 计算统计信息
    common_species = plantclef_species.intersection(plantnet_species)
    only_in_plantclef = plantclef_species - plantnet_species
    only_in_plantnet = plantnet_species - plantclef_species
    
    total_unique_species = len(plantclef_species | plantnet_species)  # 并集
    
    return {
        "plantclef_count": len(plantclef_species),
        "plantnet_count": len(plantnet_species),
        "common_count": len(common_species),
        "only_in_plantclef_count": len(only_in_plantclef),
        "only_in_plantnet_count": len(only_in_plantnet),
        "total_unique_count": total_unique_species
    }

def generate_report(stats, report_path):
    """生成分析报告"""
    # 生成报告内容
    report_content = f"""
    植物数据集分析报告

    1. 数据集概述:
    - PlantCLEF2015 数据集物种总数: {stats['plantclef_count']}
    - PlantNet300K 数据集物种总数: {stats['plantnet_count']}

    2. 共同物种和独有物种数量:
    - 共同物种数量: {stats['common_count']}
    - 仅在 PlantCLEF2015 中出现的物种数量: {stats['only_in_plantclef_count']}
    - 仅在 PlantNet300K 中出现的物种数量: {stats['only_in_plantnet_count']}

    3. 总的植物种类数量:
    - 合并后的植物种类数量: {stats['total_unique_count']}
    """
    
    # 将报告写入文件
    with open(report_path, 'w', encoding='utf-8') as f:
        f.write(report_content)
    
    print(f"分析报告已生成: {report_path}")

if __name__ == "__main__":
    plantclef_path = "plantclef2015.json"
    plantnet_path = "plantnet300k.json"
    report_path = "analysis_report.txt"
    
    # 分析物种信息
    stats = analyze_species(plantclef_path, plantnet_path)
    
    # 生成报告
    generate_report(stats, report_path) 
```
![Image](https://github.com/user-attachments/assets/bf55a0ca-f3bd-42b9-8e2b-525fb59fab00)