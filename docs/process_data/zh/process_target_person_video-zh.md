# 处理说话人视频数据集

你需要一个大约3分钟的目标人物视频来训练特定人物的postnet和基于NeRF的渲染器。（视频长度越长越好）

我们在  `data/raw/videos/May.mp4` 路径下提供了一个示例视频

## 提取所有所需的特征并打包。
先在阿里云盘中下载wav2vec2-large-xlsr-53-esperanto.zip，将其放在geneface/cpierse中，然后`unzip ***.zip`. 如果不做这一步，会导致process data中的task 2报错

需要下载hubert-large-ls960-ft，将其放在geneface/Facebook中，然后`unzip ***.zip`,如果不做这一步，会导致extract_hubert_mel_f0.py报错。

还需将Geneface/egs/datasets/videos文件夹下创建相应文件夹，文件夹名字为 VIDEO_ID如Obama_ours,文件夹下的内容可以将May中的内容全部复制过去，但是需要将每个yaml文件内的May换为相应的视频id。否则binarizer.py那一步会报错

如果处理某个视频时候出现RuntimeError: The expanded size of the tensor (50) must match the existing size (55) at non-singleton dimension 0. Target sizes: [50, 44]. Tensor sizes: [55, 44]，先确定视频是不是512*512和25fps的，如果没问题，在万兴喵影中减掉一小点视频，导出后再处理，也许就会好了。

运行如下命令行：

```
conda activate geneface
export PYTHONPATH=./
export VIDEO_ID=May
sudo chmod -R 777  data_gen/nerf
CUDA_VISIBLE_DEVICES=0 data_gen/nerf/process_data.sh $VIDEO_ID
```

如果上面的步骤都顺利完成，你可以在 `data/binary/videos/May`路径下看到处理好的目标说话人视频的数据集。
