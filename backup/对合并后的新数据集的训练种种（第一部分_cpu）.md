首先我是在Featurize上租的云服务器，主要租用了3060和纯cpu这两种。

![Image](https://github.com/user-attachments/assets/ea871a11-e9cc-4b3e-80a0-935d21450625)

参考了同济子豪兄的图像分类教程。

使用的主要是ResNet50来进行模型训练。

为什么我要租用两个服务器呢？其实主要是为了节省时间。

## 一、在cpu上主要进行的是数据的统计的相关工作
### 1、在可视化文件夹中的图像方面

（另外学到一招，就是如果想得到绝对路径，就不断地ls,cd...调到对应的那个路径下面，然后输入pwd）

![Image](https://github.com/user-attachments/assets/ff500094-f96a-4520-945e-c2c1633c1737)

![Image](https://github.com/user-attachments/assets/b420d862-0d24-46c7-959e-ac42545b1206)

![Image](https://github.com/user-attachments/assets/5dd2894b-3174-4bb7-941e-ab7061cb1e31)

### 2、在图像分类数据集探索统计方面

![Image](https://github.com/user-attachments/assets/72e87605-20ca-4473-9dfc-bd05dad119e0)
这里也学到一招，就是使用华为云的OBS服务器可以存储一些资源文件。我买了一个包年的，50G的，花了9块钱，然后买了一个月的流量包是12块钱。华为可以用OBS Browser，登录的时候注意IMA登录的话不要用手机号，而是使用用户名。

![Image](https://github.com/user-attachments/assets/57688ed6-4ba5-445f-b3d1-743f9c7a5af5)

![Image](https://github.com/user-attachments/assets/17fe1a3e-004a-468c-a602-259d5797e745)

![Image](https://github.com/user-attachments/assets/b5053faa-a60b-412c-b861-86413891e59b)

![Image](https://github.com/user-attachments/assets/dfa09bf4-d200-4f9e-ae8b-d0743a8d337e)

![Image](https://github.com/user-attachments/assets/9e873d76-9392-4d3d-b9ff-30ed25f8edc5)

![Image](https://github.com/user-attachments/assets/9ae10749-97fb-4e11-bb5f-e87904c660e1)

![Image](https://github.com/user-attachments/assets/3313fed1-6ef8-40c2-9520-b5777a921b33)

### 3、在划分训练集和测试集方面

![Image](https://github.com/user-attachments/assets/11a2f521-8482-4dcb-82aa-9c66c4085b98)

![Image](https://github.com/user-attachments/assets/3782b5d2-6f0b-49f9-a9ce-0a684a5e651b)

也学到一个看解压到哪里的小技巧：

![Image](https://github.com/user-attachments/assets/4ff7e1cf-194a-43fc-8328-81ea584a6f82)

不断地刷新，就能知道现在解压到多少了。大数据集适用的小技巧。

![Image](https://github.com/user-attachments/assets/0e3456e0-9b33-4eeb-9d8e-fdf490d7f092)

创建训练集文件夹和测试集文件夹：

![Image](https://github.com/user-attachments/assets/4070866b-c9f6-4c15-b816-54fb8784de6e)

划分训练集、测试集，移动文件：
```markdown
df = pd.DataFrame()

print('{:^18} {:^18} {:^18}'.format('类别', '训练集数据个数', '测试集数据个数'))

for fruit in classes: # 遍历每个类别

    # 读取该类别的所有图像文件名
    old_dir = os.path.join(dataset_path, fruit)
    images_filename = os.listdir(old_dir)
    random.shuffle(images_filename) # 随机打乱

    # 划分训练集和测试集
    testset_numer = int(len(images_filename) * test_frac) # 测试集图像个数
    testset_images = images_filename[:testset_numer]      # 获取拟移动至 test 目录的测试集图像文件名
    trainset_images = images_filename[testset_numer:]     # 获取拟移动至 train 目录的训练集图像文件名

    # 移动图像至 test 目录
    for image in testset_images:
        old_img_path = os.path.join(dataset_path, fruit, image)         # 获取原始文件路径
        new_test_path = os.path.join(dataset_path, 'val', fruit, image) # 获取 test 目录的新文件路径
        shutil.move(old_img_path, new_test_path) # 移动文件

    # 移动图像至 train 目录
    for image in trainset_images:
        old_img_path = os.path.join(dataset_path, fruit, image)           # 获取原始文件路径
        new_train_path = os.path.join(dataset_path, 'train', fruit, image) # 获取 train 目录的新文件路径
        shutil.move(old_img_path, new_train_path) # 移动文件
    
    # 删除旧文件夹
    assert len(os.listdir(old_dir)) == 0 # 确保旧文件夹中的所有图像都被移动走
    shutil.rmtree(old_dir) # 删除文件夹
    
    # 工整地输出每一类别的数据个数
    print('{:^18} {:^18} {:^18}'.format(fruit, len(trainset_images), len(testset_images)))
    
    # 保存到表格中
    df = df.append({'class':fruit, 'trainset':len(trainset_images), 'testset':len(testset_images)}, ignore_index=True)

# 重命名数据集文件夹
shutil.move(dataset_path, dataset_name+'_split')

# 数据集各类别数量统计表格，导出为 csv 文件
df['total'] = df['trainset'] + df['testset']
df.to_csv('数据量统计.csv', index=False)
```
显示结果如下：
```markdown
df = pd.DataFrame()

print('{:^18} {:^18} {:^18}'.format('类别', '训练集数据个数', '测试集数据个数'))

for fruit in classes: # 遍历每个类别

    # 读取该类别的所有图像文件名
    old_dir = os.path.join(dataset_path, fruit)
    images_filename = os.listdir(old_dir)
    random.shuffle(images_filename) # 随机打乱

    # 划分训练集和测试集
    testset_numer = int(len(images_filename) * test_frac) # 测试集图像个数
    testset_images = images_filename[:testset_numer]      # 获取拟移动至 test 目录的测试集图像文件名
    trainset_images = images_filename[testset_numer:]     # 获取拟移动至 train 目录的训练集图像文件名

    # 移动图像至 test 目录
    for image in testset_images:
        old_img_path = os.path.join(dataset_path, fruit, image)         # 获取原始文件路径
        new_test_path = os.path.join(dataset_path, 'val', fruit, image) # 获取 test 目录的新文件路径
        shutil.move(old_img_path, new_test_path) # 移动文件

    # 移动图像至 train 目录
    for image in trainset_images:
        old_img_path = os.path.join(dataset_path, fruit, image)           # 获取原始文件路径
        new_train_path = os.path.join(dataset_path, 'train', fruit, image) # 获取 train 目录的新文件路径
        shutil.move(old_img_path, new_train_path) # 移动文件
    
    # 删除旧文件夹
    assert len(os.listdir(old_dir)) == 0 # 确保旧文件夹中的所有图像都被移动走
    shutil.rmtree(old_dir) # 删除文件夹
    
    # 工整地输出每一类别的数据个数
    print('{:^18} {:^18} {:^18}'.format(fruit, len(trainset_images), len(testset_images)))
    
    # 保存到表格中
    df = df.append({'class':fruit, 'trainset':len(trainset_images), 'testset':len(testset_images)}, ignore_index=True)

# 重命名数据集文件夹
shutil.move(dataset_path, dataset_name+'_split')

# 数据集各类别数量统计表格，导出为 csv 文件
df['total'] = df['trainset'] + df['testset']
df.to_csv('数据量统计.csv', index=False)
```
清理空文件：
```markdown
(base) ➜  PlantImage find /home/featurize/data/PlantImage_split/val -type d -empty -delete
```

### 4、统计图像尺寸、比例分布（未完待续）

