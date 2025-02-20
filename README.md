首先确保你的python版本在3.8至3.12版本内。

请访问([Python](https://www.python.org/downloads/))官网进行下载所需版本并安装。

此笔记主要应用到([PyTorch](https://pytorch.org/))作为学习与开发应用，如了解更多请访问它的官网。

## 安装必备库

pytorch库（PyTorch是一个流行的深度学习框架）

    pip install torch torchvision torchaudio #安装

cv2库（cv2是OpenCV的Python接口，用于图像处理和计算机视觉任务）

    pip install opencv-python  #安装

tqdm库（tqdm   是一个用于显示进度条的库，可不需要，前提是在本脚本中删除它的函数）
 
    pip install tqdm
# 全部库
    
    import os
    import cv2
    import numpy as np
    from shutil import copyfile
    from tqdm import tqdm
 
    def create_folders(base_folder):



    
# 自动创建主文件夹
     if not os.path.exists(base_folder):  # 检查主文件夹是否存在，如不存在则会自动创建
        os.makedirs(base_folder)

![屏幕截图 2025-02-20 183202](https://github.com/user-attachments/assets/e7aec5f6-82da-4f8e-882b-bf825acb1ae5)

    
    for folder in ['red', 'green', 'blue']: # 在主文件夹下创建red, green, blue 子文件夹
        folder_path = os.path.join(base_folder, folder)
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)

![屏幕截图 2025-02-20 183351](https://github.com/user-attachments/assets/17273240-991e-4c26-917c-cfb359a80ccb)
   

# 计算图片平均值
 
    def dominant_color(image):

     avg_color_per_row = np.average(image, axis=0)
     avg_color = np.average(avg_color_per_row, axis=0)
    
    # 获取颜色通道的索引（BGR）
    dominant = np.argmax(avg_color)
    
    # 将索引转换为颜色名称
    if dominant == 0:
        return 'blue'
    elif dominant == 1:
        return 'green'
    elif dominant == 2:
        return 'red'


    def classify_images(img_folder, base_folder):

    create_folders(base_folder)
    
# 遍历图片文件夹
    for img_name in tqdm(os.listdir(img_folder), desc="Processing images"):
        img_path = os.path.join(img_folder, img_name)
        
# 读取图片
        image = cv2.imread(img_path)
        if image is None:
            print(f"Failed to load image: {img_path}")
            continue
        
# 判断主要颜色
        color = dominant_color(image)
        
# 将图片复制到对应文件夹
        target_folder = os.path.join(base_folder, color, img_name)
        copyfile(img_path, target_folder)

    if __name__ == "__main__":
       img_folder = 'img'  # 图片文件夹路径
       base_folder = 'class'  # 主文件夹路径
       classify_images(img_folder, base_folder)
       print("Image classification completed!")




## The end   下面进行测试环节

1、如没有就请自行创建一个名为img的文件夹（注：要和pytorch-img.py文件创建到同一个目录下）

2、在img文件夹中放入n张分别含有红、绿、蓝元素的图片(一定要是jpg格式的)如图所示：)

![屏幕截图 2025-02-20 183604](https://github.com/user-attachments/assets/e812304d-c075-43b6-9f18-e05dcbc3bb89)

3、我们来执行pytorch-img.py文件 windows系统下

所在目录右键在cmd中打开，执行：

     python pytorch-img.py


linux系统下所在目录执行：

    python3 pytorch-img.py

4、如图所示，当进度条加载完成后表明图片已经分类好了

![屏幕截图 2025-02-20 185053](https://github.com/user-attachments/assets/2165ed33-bbf3-4c15-8e3b-4c05882c001d)



5、接着我们分别查看class中的red、green、bule三个目录，所见 您放到img文件中的图片已经帮您按三色源分类好

![屏幕截图 2025-02-20 185504](https://github.com/user-attachments/assets/e256a763-37b3-413a-bdc1-ad3a9430c533)



## 本笔记就到这里了，欢迎各大IT友前来讨论。

编辑作者：困困
