首先下载数据集
从kaggle下载所需数据集。[来源](https://www.kaggle.com/datasets/noahbadoa/plantnet-300k-images)。
然后本地解压。大概需要较长时间。
解压出来的文件结构如下：

![Image](https://github.com/user-attachments/assets/1fb6a080-8ee9-4e4f-ad85-5920fc25aef1)
点开，会发现内部的文件名是以数字排序。

![Image](https://github.com/user-attachments/assets/c7355930-1885-414f-b238-8ee4d7e7d8e1)

但是存在一个数字和名称的映射表：

![Image](https://github.com/user-attachments/assets/94d9aa10-f2dd-49a9-9d06-65704b99cb11)

编写如下python文件，读取映射表，转换文件名：
```markdown
import os
import json
import sys

def rename_folders():
    # 获取当前脚本所在目录
    current_dir = os.path.dirname(os.path.abspath(__file__))
    
    # 读取物种名称映射文件
    json_path = os.path.join(current_dir, 'plantnet300K_species_names.json')
    try:
        with open(json_path, 'r', encoding='utf-8') as f:
            species_names = json.load(f)
    except FileNotFoundError:
        print(f"错误: 找不到文件 {json_path}")
        return
    except json.JSONDecodeError:
        print("错误: JSON文件格式不正确")
        return
    
    # 需要处理的文件夹列表
    folders = ['images_test', 'images_train', 'images_val']
    
    # 遍历每个主文件夹
    for folder in folders:
        folder_path = os.path.join(current_dir, folder)
        if not os.path.exists(folder_path):
            print(f"警告: {folder_path} 文件夹不存在")
            continue
            
        print(f"处理文件夹: {folder_path}")
        
        # 遍历子文件夹（物种ID文件夹）
        for species_id in os.listdir(folder_path):
            old_path = os.path.join(folder_path, species_id)
            
            # 确保是文件夹而不是文件
            if not os.path.isdir(old_path):
                continue
                
            # 获取对应的物种名称
            if species_id in species_names:
                # 将物种名称中的下划线替换为空格，处理其他非法字符
                species_name = species_names[species_id].replace('_', ' ')
                species_name = species_name.replace('/', ' ').replace('\\', ' ')
                # 只保留字母、数字、空格和连字符
                species_name = ''.join(c if c.isalnum() or c in ('-', ' ') else ' ' for c in species_name)
                # 合并多个连续的空格
                species_name = ' '.join(species_name.split())
                
                new_path = os.path.join(folder_path, species_name)
                
                try:
                    if os.path.exists(new_path):
                        print(f"警告: 目标路径已存在，跳过: {new_path}")
                        continue
                        
                    os.rename(old_path, new_path)
                    print(f"已重命名: {old_path} -> {new_path}")
                except Exception as e:
                    print(f"重命名失败 {old_path}: {str(e)}")
            else:
                print(f"警告: 未找到ID {species_id} 对应的物种名称")

if __name__ == "__main__":
    print("开始重命名文件夹...")
    print(f"当前工作目录: {os.getcwd()}")
    rename_folders()
    print("重命名完成！") 

```
从总目录打开cmd，输入
```markdown
python rename_folders.py
```
会开始转换过程：

![Image](https://github.com/user-attachments/assets/3fed6cae-c541-46e5-8521-2942c5b606b3)

对重复植物进行去重，保证精准。

![Image](https://github.com/user-attachments/assets/8270bd6e-f94a-4727-a293-4c8f7a1466c1)

![Image](https://github.com/user-attachments/assets/6a7b6aaa-a68d-4356-a7ba-a32bba02777c)

转换后的文件夹如下：

![Image](https://github.com/user-attachments/assets/9e42be16-0bc0-4d0d-b9d5-6b3006f64f6d)

