# CNNTraining
This repository provides the necessary instructions for training a Tensorflow Convolutional Neural Network , for the Purpose of identifying Platanus hispanica and Celtis australis trees in the streets of Barcelona, Spain.

This workflow has been developed using Gilbert Tanner's [tutorial](https://gilberttanner.com/blog/creating-your-own-objectdetector) on how to use Tensorflow for creating a live object detection model.

## Contents
In order to create a CNN using Tensorflow's object_detection API, you need (and we provide in this repo):
1. The **image files** for training and testing.
These .jpg files can be found under 'images' and are further separated in 'train' and 'test' folders.
2. An **.xml file** for each of the abovementioned images, which contain the data for locating the objects of interest within the images.
These files can be found in the same folder as their respective .jpg image.
3. A **labelmap**, that contains the list of classes of the objects of interest.
In our case the file is called labelmap.pbtxt and can be found under 'training' folder. It contains only 2 classes, Platanus_x_hispanica and Celtis_australis.
4. A **.config file** that describes the pipeline of the CNN that is going to be used. In our case is called faster_rcnn_inception_v2_pets.config (as we are using Faster R-CNN architecture) and can be found under 'training'.
5. **Folder faster_rcnn_inception_v2_coco_2018_01_28** which containes the necessary information to build a Faster R-CNN model.
6. **Python scripts** xml_to_csv.py and generate_tfrecords.py, that were initially found in [datitran/raccoon_dataset](https://github.com/datitran/raccoon_dataset) repository and have been modified to fit this process' s needs.
7. A **'time_saver' folder**, that contains files that can be created during the process, but can also be found there, in case you want to save time and skip some steps. These files are: 
    * .csv files, created by xml_to_csv.py
    * .record files, created by generate_tfrecords.py
8. Finally, under 'doc_imgs', we have placed documentation images, whose only purpose is to support this tutorial and take no part in the actual process.


## Prerequisites
To build a Tensorflow object detection model, you must have the following prerequisites installed on your system:
1. Environment with Python > 3.6.10.
2. The following commands will install almost all packages that you will need:
```Bash
pip install tensorflow==1.15.2
pip install pandas
```
3. Download or git clone [this](https://github.com/tensorflow/models) repository.

The instructions above should end in acquiring locally the tree-structured project directories of Tensorflow. \
Example: \
-models \
&nbsp;&nbsp;&nbsp;&nbsp;-research \
&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp; -object_detection \
&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;... \
&nbsp;&nbsp;&nbsp;&nbsp; ...

4. Navigate to models/research directory and run
```Bash
pip install .
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
```
## How to use this project

1. Download or git clone the repository and copy 'CNNTraining' under object_detection. Now your file structure should be: \
-models \
&nbsp;&nbsp;&nbsp;&nbsp;-research \
&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp; -object_detection \
&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; -CNNTraining \
&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; ... \
&nbsp;&nbsp;&nbsp;&nbsp; ...


1. Navigate to models/research/object_detection/CNNTraining and run
```Bash
python xml_to_csv.py
```
This will create 2 new csv files under images/ that are combine all the xml files that have been created for each image. Note that you get separate files for training and testing the CNN.
You can skip this step by using the ready csv files provided by this repo. (just copy and paste them under images)

2. 
```Bash
python generate_tfrecords.py --csv_input=images/train_labels.csv --image_dir=images/train --output_path=train.record

python generate_tfrecords.py --csv_input=images/test_labels.csv --image_dir=images/test --output_path=test.record
```
This process turns csv files into a format that can be manipulated by tensorflow. Just copy-paste to go further. OR copy-paste and proceed to step 3.
 
 
3. Navigate to object_detection
```Bash
cd ..
python model_main.py --logtostderr --model_dir=CNNTraining/training/ --pipeline_config_path=CNNTraining/training/faster_rcnn_inception_v2_pets.config
```

4. check tensorboard to stop the process
 
5. note the checkpoint before exporting the inference graph
 python export_inference_graph.py --input_type image_tensor --pipeline_config_path prj_training/faster_rcnn_inception_v2_pets.config --trained_checkpoint_prefix prj_training/model.ckpt-3699 --output_directory inference_graph
