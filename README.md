首先确保你的python版本在3.8至3.12版本内。

请访问([Python](https://www.python.org/downloads/))官网进行下载所需版本并安装。

##安装必备库

pytorch库

    pip install pytorch #安装

cv2库（cv2是OpenCV的Python接口，用于图像处理和计算机视觉任务）

    pip install opencv-python  #安装

tqdm库（tqdm   是一个用于显示进度条的库，可不需要，前提是在本脚本中删除它的函数）
 
    pip install tqdm
##全部库
    
    import os
    import cv2
    import numpy as np
    from shutil import copyfile
    from tqdm import tqdm
 
    def create_folders(base_folder):

    
## 自动创建主文件夹
     if not os.path.exists(base_folder):  # 检查主文件夹是否存在，如不存在则会自动创建
        os.makedirs(base_folder)
    
    for folder in ['red', 'green', 'blue']: # 在主文件夹下创建red, green, blue 子文件夹
        folder_path = os.path.join(base_folder, folder)
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)

## 计算图片平均值
 
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
    
## 遍历图片文件夹
    for img_name in tqdm(os.listdir(img_folder), desc="Processing images"):
        img_path = os.path.join(img_folder, img_name)
        
## 读取图片
        image = cv2.imread(img_path)
        if image is None:
            print(f"Failed to load image: {img_path}")
            continue
        
## 判断主要颜色
        color = dominant_color(image)
        
## 将图片复制到对应文件夹
        target_folder = os.path.join(base_folder, color, img_name)
        copyfile(img_path, target_folder)

    if __name__ == "__main__":
       img_folder = 'img'  # 图片文件夹路径
       base_folder = 'class'  # 主文件夹路径
       classify_images(img_folder, base_folder)
       print("Image classification completed!")
