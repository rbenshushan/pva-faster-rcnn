## PVANet: Lightweight Deep Neural Networks for Real-time Object Detection
by Sanghoon Hong, Byungseok Roh, Kye-hyeon Kim, Yeongjae Cheon, Minje Park (Intel Imaging and Camera Technology)
Presented in [EMDNN2016](http://allenai.org/plato/emdnn/), a NIPS2016 workshop ([arXiv link](https://arxiv.org/abs/1611.08588))

### Introduction

This repository is a fork from [py-faster-rcnn](https://github.com/rbgirshick/py-faster-rcnn) and demonstrates the performance of PVANet.

You can refer to [py-faster-rcnn README.md](https://github.com/rbgirshick/py-faster-rcnn/blob/master/README.md) and [faster-rcnn README.md](https://github.com/ShaoqingRen/faster_rcnn/blob/master/README.md) for more information.

### Desclaimer

This repository doesn't contain our up-to-date models and codes.
I'll be updated by the end of December.

Please note that this repository doesn't contain our in-house codes used in the published article.
- This version of py-faster-rcnn is slower than our in-house runtime code due to the image pre-processing code written in Python (+9 ms) and some poorly implemented parts in Caffe (+5 ms).
- PVANet was trained by our in-house deep learning library, not by this implementation.
- There might be a tiny difference in VOC2012 test results, because some hidden parameters in py-faster-rcnn may be set differently with ours.

### Citing PVANet

If you want to cite this work in your publication:
```
@article{hong2016pvanet,
  title={{PVANet}: Lightweight Deep Neural Networks for Real-time Object Detection},
  author={Hong, Sanghoon and Roh, Byungseok and Kim, Kye-Hyeon and Cheon, Yeongjae and Park, Minje},
  journal={arXiv preprint arXiv:1611.08588},
  year={2016}
}
```

### Installation

1. Clone the Faster R-CNN repository
  ```Shell
  # Make sure to clone with --recursive
  git clone --recursive https://github.com/sanghoon/pva-faster-rcnn.git
  ```

2. We'll call the directory that you cloned Faster R-CNN into `FRCN_ROOT`. Build the Cython modules
    ```Shell
    cd $FRCN_ROOT/lib
    make
    ```

3. Build Caffe and pycaffe
    ```Shell
    cd $FRCN_ROOT/caffe-fast-rcnn
    # Now follow the Caffe installation instructions here:
    #   http://caffe.berkeleyvision.org/installation.html
    # For your Makefile.config:
    #   Uncomment `WITH_PYTHON_LAYER := 1`

    cp Makefile.config.example Makefile.config
    make -j8 && make pycaffe
    ```

    ```
    mkdir build
    cd build
    cmake ..
    make all
    ```
    
4. Download PVANet caffemodels
    ```Shell
    cd $FRCN_ROOT
    ./models/pvanet/download_models.sh
    ```
  - If it does not work,
    1. Download [full/test.model](https://drive.google.com/open?id=0BwFPOX3S4VcBd3NPNmI1RHBZNkk) and move it to ./models/pvanet/full/
    2. Download [comp/test.model](https://drive.google.com/open?id=0BwFPOX3S4VcBODJkckhudE1NeGM) and move it to ./models/pvanet/comp/

5. (Optional) Download original caffemodels (without merging batch normalization and scale layers)
    ```Shell
    cd $FRCN_ROOT
    ./models/pvanet/download_original_models.sh
    ```
  - If it does not work,
    1. Download [full/original.model](https://drive.google.com/open?id=0BwFPOX3S4VcBUW1OS1Fva3VKZ1E) and move it to ./models/pvanet/full/
    2. Download [comp/original.model](https://drive.google.com/open?id=0BwFPOX3S4VcBdVZuX3dQRzFjU1k) and move it to ./models/pvanet/comp/

6. (Optional) Download ImageNet pretrained models
    ```Shell
    cd $FRCN_ROOT
    ./models/pvanet/download_imagenet_models.sh
    ```
  - If it does not work,
    1. Download [imagenet/original.model](https://drive.google.com/open?id=0BwFPOX3S4VcBd1VtRzdHa1NoN1k) and move it to ./models/pvanet/imagenet/
    2. Download [imagenet/test.model](https://drive.google.com/open?id=0BwFPOX3S4VcBWnI0VHRzZWh6bFU) and move it to ./models/pvanet/imagenet/

7. (Optional) Download PVANet-lite models
    ```Shell
    cd $FRCN_ROOT
    ./models/pvanet/download_lite_models.sh
    ```
  - If it does not work,
    1. Download [lite/original.model](https://drive.google.com/open?id=0BwFPOX3S4VcBc1ZEQldZTlZKN00) and move it to ./models/pvanet/lite/
    2. Download [lite/test.model](https://drive.google.com/open?id=0BwFPOX3S4VcBSWg2MlpGcWlQeHM) and move it to ./models/pvanet/lite/

### Models

1. PVANet
  - `./models/pvanet/full/test.pt`: For testing-time efficiency, batch normalization (w/ its moving averaged mini-batch statistics) and scale (w/ its trained parameters) layers are merged into the corresponding convolutional layer.
  - `./models/pvanet/full/original.pt`: Original network structure.

2. PVANet (compressed)
  - `./models/pvanet/comp/test.pt`: Compressed network w/ merging batch normalization and scale.
  - `./models/pvanet/comp/original.pt`: Original compressed network structure.

3. PVANet (ImageNet pretrained model)
  - `./models/pvanet/imagenet/test.pt`: Classification network w/ merging batch normalization and scale.
  - `./models/pvanet/imagenet/original.pt`: Original classification network structure.

4. PVANet-lite
  - `./models/pvanet/lite/test.pt`: Compressed network w/ merging batch normalization and scale.
  - `./models/pvanet/lite/original.pt`: Original compressed network structure.


### How to run the demo

1. Download PASCAL VOC 2007 and 2012
  - Follow the instructions in [py-faster-rcnn README.md](https://github.com/rbgirshick/py-faster-rcnn#beyond-the-demo-installation-for-training-and-testing-models)


### Beyond the demo: installation for training and testing models
1. Download the training, validation, test data and VOCdevkit

	```Shell
	wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
	wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
	wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCdevkit_08-Jun-2007.tar
	```

2. Extract all of these tars into one directory named `VOCdevkit`

	```Shell
	tar xvf VOCtrainval_06-Nov-2007.tar
	tar xvf VOCtest_06-Nov-2007.tar
	tar xvf VOCdevkit_08-Jun-2007.tar
	```

3. It should have this basic structure

	```Shell
  	$VOCdevkit/                           # development kit
  	$VOCdevkit/VOCcode/                   # VOC utility code
  	$VOCdevkit/VOC2007                    # image sets, annotations, etc.
  	# ... and several other directories ...
  	```

4. Create symlinks for the PASCAL VOC dataset

	```Shell
    cd $FRCN_ROOT/data
    ln -s $VOCdevkit VOCdevkit2007
    ```

2. PVANet on PASCAL VOC 2007
  ```Shell
  cd $FRCN_ROOT
  ./tools/test_net.py --gpu 0 --def models/pvanet/full/test.pt --net models/pvanet/full/test.model --cfg models/pvanet/cfgs/submit_160715.yml
  ```

3. PVANet (compressed)
  ```Shell
  cd $FRCN_ROOT
  ./tools/test_net.py --gpu 0 --def models/pvanet/comp/test.pt --net models/pvanet/comp/test.model --cfg models/pvanet/cfgs/submit_160715.yml
  ```

4. (Optional) ImageNet classification
  ```Shell
  cd $FRCN_ROOT
  ./caffe-fast-rcnn/build/tools/caffe test -gpu 0 -model models/pvanet/imagenet/test.pt -weights models/pvanet/imagenet/test.model -iterations 1000
  ```

5. (Optional) PVANet-lite
  ```Shell
  cd $FRCN_ROOT
  ./tools/test_net.py --gpu 0 --def models/pvanet/lite/test.pt --net models/pvanet/lite/test.model --cfg models/pvanet/cfgs/submit_160715.yml
  ```

### Expected results

- PVANet: 83.85% mAP
- PVANet (compressed): 82.90% mAP
- ImageNet classification: 68.998% top-1 accuracy, 88.8902% top-5 accuracy, 1.28726 loss
- PVANET-lite: 79.10% mAP
