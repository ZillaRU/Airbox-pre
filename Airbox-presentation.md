# Airbox大模型演示

*zhongying.ru@sophgo.com*

## 安装、部署操作

1. 如果是原厂发来的裸机，需要刷机；若不是裸机，跳过这步。

    刷机步骤：
    1. 准备一张不小于32GB的sd卡和一个读卡器。格式化sd卡为FAT32格式，参考[这里的格式化操作](https://support.bitmain.com/hc/zh-cn/articles/9856777513113-T19-%E5%8D%A1%E5%88%B7%E6%95%99%E7%A8%8B)。
    2. [下载刷机包](https://pan.baidu.com/s/1gQO6b0WRPfxua3rYkcxwHA?pwd=swje)，这个刷机包里有llama、qwen、emotivoice、imagesearch、chatglm-int4、lcm的代码和模型以及CASAOS。如果需要其他应用，可以`git clone xxxx`，xxxx为github仓库链接，下面应用列表的code就是链接，环境配置和具体操作参考链接内的readme文档。下载完成后解压缩，将如图这些文件（BOOT等等）直接复制到sd卡的根目录下。![刷机文件](4dab308d497d98e93411235c7b257861.jpg)
    3. 将sd卡插在盒子的micro sd口；连接电源线，按PWR到绿灯亮起再松开；等待50分钟。到时间后，断电拔出sd卡。

2. 连接电源线，按PWR到绿灯亮起再松开，听到风扇声音就是启动好了。![连接电源线和网线](b3fe38ac41db07e5ce90a7d76f792104.jpg)
3. 如图连接网线。如果是首次使用网线连接盒子和电脑，需要修改本机ip为`192.168.150.2`。![修改本机ip](f5f201791b3e64dd2d848be21cb92794.jpg)
4. 打开terminal，输入`ssh linaro@192.168.150.1`，
密码`linaro`。

5. 风扇太吵的话输入

   ```sh
   sudo busybox devmem 0x50029000 32 0x500
   sudo busybox devmem 0x50029004 32 0xfa0
   ```
## BM1684X大模型能力展示

* 应用运行中，需要退出的话，直接打开对应的后台terminal窗口，按`Ctrl + C`。

1. TTS文本转语音([code](https://github.com/ZillaRU/EmotiVoice-TPU))

   - 支持千种音色
   - 多样化情感
   - 中英文
   - 生成120s语音仅需3s
   - 一行命令运行：`cd /data/EmotiVoice-TPU && python3 demo-page.py`
2. 文生图、图生图
   - SD v1.5、v2.1([code](https://github.com/forechoandlook/aigc))
   - SD XL([code](https://github.com/ZillaRU/SDXL-tpu))
   - LCM加速，单芯<2s出图，双芯加速更快 `cd /data/lcm && bash run.sh`
   - 出图可控性：ControlNet、Lora等增强出图可控性的扩展也有适配
   - 图生图-->语义超分

3. 文搜图，图搜图([code](https://github.com/ZillaRU/ImageSearch-tpu))

   - 非打标签，自然语言描述
   - 亿图秒搜（多模态大模型 + 向量数据库）
   - 一行命令运行：`cd /data/ImageSearch-tpu && streamlit run app.py CH/EN`
4. 文 / 图 搜视频([code](https://github.com/ZillaRU/VideoSearch-tpu))

   - 自然语言描述
   - 精准定位
   - 场景切分+多模态大模型+向量数据库
   - 一行命令运行：`cd /data/VideoSearch-tpu`

5. ChatDoc私域知识管理([code](https://github.com/zhengorange/chatdoc))
   - **通用知识+私域知识** 问答、chatbot聊天交互
   - LLM模型基座 + langchain + 文本向量化 + 向量数据库
   - 已适配基座：通义千问7B（[code](https://github.com/sophgo/Qwen-TPU)）、llama-7B、chatglm2-6B
   - 一行命令运行：`cd /data/chatdoc && bash run_emb_tpu.sh`

6. 图像/视频的语义分割([code](https://github.com/ZillaRU/AnnoSeg))

   - 基于提示的任意物体分割
   - **像素级标注**: 结合检测模型（YOLO / GroundingDINO）
   - 一行命令运行：`cd /data/AnnoSeg && python3 app.py --det_method groundingdino或yolov8s`

7. 超分辨率

    - 图像超分、视频超分
8. 数字人([code](https://github.com/ZillaRU/SadTalker-tpu))

   - 一张人像 + 文本 = talking video
9.  CASAOS开源家庭云系统
    
    浏览器访问`192.168.150.1:81`即可，用户名和密码都是`linaro`。
10. Whisper音频转文字([code](https://github.com/JKay0327/whisper-TPU_py))

    - 多语言识别、转录
    - -- +LLM ->：会议纪要


## 开源工具链支持

使用[TPU-MLIR](https://tpumlir.org/)算能的开源工具链，可自己完成

- **模型转换**：caffe、torch、onnx等格式模型转换为芯片支持的bmodel格式
- FP16、int8等精度的**量化加速和校准**
- **加算子**
  
提供[快速入门文档](https://tpumlir.org/docs/quick_start/index.html)、[开发手册](https://tpumlir.org/docs/developer_manual/index.html)和[视频教程](https://tpumlir.org/docs/videos.html)。
