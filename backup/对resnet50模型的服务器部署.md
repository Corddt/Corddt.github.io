经过训练，目前有了.pth的模型，另外还有.npy文件。现在尝试通过两个文件将模型部署到服务器上去。选择的服务器是Featurize服务器。
具体的.pth模型训练流程参考这个[视频](https://www.bilibili.com/video/BV1Ng411C7WY?vd_source=348b4747c7e04cd236a0b05918dadc36&spm_id_from=333.788.videopod.sections)。

首先，获取json文件。获取json文件的方式是将视频里的那个idx_to_labels.npy文件通过python脚本转化为json。
python脚本如下：
```python
import numpy as np
import json

# 加载npy文件
labels_to_idx = np.load('labels_to_idx.npy', allow_pickle=True).item()
idx_to_labels = np.load('idx_to_labels.npy', allow_pickle=True).item()

# 转换为JSON格式并保存
with open('labels_to_idx.json', 'w', encoding='utf-8') as f:
    json.dump(labels_to_idx, f, ensure_ascii=False, indent=4)

with open('idx_to_labels.json', 'w', encoding='utf-8') as f:
    json.dump(idx_to_labels, f, ensure_ascii=False, indent=4)

# 查看转换后的内容
print("labels_to_idx的内容:")
print(json.dumps(labels_to_idx, ensure_ascii=False, indent=4))
print("\nidx_to_labels的内容:")
print(json.dumps(idx_to_labels, ensure_ascii=False, indent=4))
```
对.pth的转换，参考这个[视频](https://www.bilibili.com/video/BV1cM4y187Xc?vd_source=348b4747c7e04cd236a0b05918dadc36&spm_id_from=333.788.videopod.sections)。通过这个视频，就可以将.pth转换为ONNX格式。

有了这两个之后，就可以正式进行部署了。
首先在Feature上新开一个服务器。

![Image](https://github.com/user-attachments/assets/5a32e5cb-547f-4b63-8df5-4428f0103548)

![Image](https://github.com/user-attachments/assets/18164f09-97b7-4212-a0c1-2d4758fad39c)

新建文件夹，我这里命名为Plant_ONNX。

![Image](https://github.com/user-attachments/assets/30008bf1-9af6-4753-83d5-205dc4af76fa)

文件夹里有以下文件：

![Image](https://github.com/user-attachments/assets/4ee07f03-7630-466a-ad9a-c04f146eee1f)

app.py的具体代码：
```python
from fastapi import FastAPI, File, UploadFile
from fastapi.middleware.cors import CORSMiddleware
import numpy as np
import onnxruntime as ort
from PIL import Image
import io
import json
import scipy.special  # 添加scipy.special用于softmax函数

app = FastAPI(title="植物分类API 🌿")

# 允许跨域请求
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# 加载ONNX模型
model_path = "resnet50_plant1391.onnx"
# 修改后
ort_session = ort.InferenceSession(
    model_path,
    providers=['CPUExecutionProvider']  # 或者根据报错信息使用 ['AzureExecutionProvider', 'CPUExecutionProvider']
)

# 加载标签映射
with open("resnet50_plant1391.json", "r", encoding="utf-8") as f:
    idx_to_labels = json.load(f)

# 图像预处理函数
def preprocess_image(image):
    # 调整图像大小为256x256
    image = image.resize((256, 256))
    # 转换为numpy数组并明确指定float32类型
    img_array = np.array(image).astype(np.float32)
    # 转换为BatchxChannelxHeightxWidth格式
    img_array = img_array.transpose(2, 0, 1)
    # 增加批次维度
    img_array = img_array[np.newaxis, ...]
    # 归一化(假设模型期望输入范围为0-1)
    img_array = img_array / 255.0
    
    # 确保使用float32类型的数组进行归一化
    mean = np.array([0.485, 0.456, 0.406], dtype=np.float32).reshape(1, 3, 1, 1)
    std = np.array([0.229, 0.224, 0.225], dtype=np.float32).reshape(1, 3, 1, 1)
    img_array = (img_array - mean) / std
    
    # 最后再次确保类型是float32
    return img_array.astype(np.float32)

@app.post("/predict")
async def predict(file: UploadFile = File(...)):
    try:
        # 读取上传的图片
        content = await file.read()
        image = Image.open(io.BytesIO(content)).convert("RGB")
        
        # 预处理图像
        input_data = preprocess_image(image)
        
        # 运行推理
        outputs = ort_session.run(None, {"input": input_data})
        
        # 将logits转换为概率 - 使用softmax函数
        probabilities = scipy.special.softmax(outputs[0][0])
        
        # 处理结果
        class_idx = np.argmax(probabilities)
        confidence = float(probabilities[class_idx])
        class_name = idx_to_labels.get(str(class_idx), "未知类别")
        
        # 获取前5个最可能的类别
        top5_indices = np.argsort(probabilities)[-5:][::-1]
        top5_results = [
            {
                "class_name": idx_to_labels.get(str(idx), "未知类别"),
                "confidence": float(probabilities[idx])
            }
            for idx in top5_indices
        ]
        
        return {
            "prediction": class_name,
            "confidence": confidence,
            "top5_results": top5_results
        }
    
    except Exception as e:
        return {"error": str(e)}

@app.get("/")
def read_root():
    return {"message": "植物分类API服务正在运行，请使用/predict端点上传图片进行识别"}

# 获取所有支持的植物类别
@app.get("/categories")
def get_categories():
    return {"categories": list(idx_to_labels.values())}
```
requirements.txt的具体代码：
```
fastapi==0.104.1
uvicorn==0.24.0
onnxruntime==1.16.0
numpy==1.24.3
Pillow==10.0.1
python-multipart==0.0.6
scipy==1.11.3
```
start.sh的具体代码：
```
#!/bin/bash
pip install -r requirements.txt
uvicorn app:app --host 0.0.0.0 --port 8000
```
现在开始运行。
首先给予权限。然后运行。

![Image](https://github.com/user-attachments/assets/81196ea3-6d06-4f4b-8dc6-abbb043e18cf)

现在外界还无法访问。
现在去开放端口。

![Image](https://github.com/user-attachments/assets/1abd9e20-7bf5-4f4d-85ad-01e7670c82e6)

现在就可以正常访问了。
通过apipost进行测试：

![Image](https://github.com/user-attachments/assets/5cb41352-7327-44e2-8a40-35d538693c50)

可以看见服务器正常返回结果。
