## Naomi
[![Build Status](https://travis-ci.org/imamdigmi/naomi.svg?branch=master)](https://travis-ci.org/imamdigmi/naomi)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)

## Setup

Clone models repostory
```
git clone https://github.com/tensorflow/models.git
```

Compile Proto Buffer (protobof) inside `models/research` directory
```
protoc object_detection/protos/*.proto --python_out=.
```

And then export $PYTHONPATH variable inside `models/research` directory
```
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
```

Setuptools
```
sudo python3 setup.py install
```
---

## Graph
Loss
![Loss](screenshots/losses.png "Loss")

Batch
![Batch](screenshots/batch.png "Batch")

Box Predictor (Historgram)
![Box Predictor](screenshots/box-predictor-histogram.png "Box Predictor")

Steps
![Global Step](screenshots/global-step.png "Global Step")

---

## Usage
Convert dataset meta labels xml to csv

```
python3 xml_to_csv.py
```

Convert csv dataset labels to TFRecord file format

```
python3 generate_tfrecord.py --type=train --csv_input=data/train_labels.csv  --output_path=data/train.record
python3 generate_tfrecord.py --type=test --csv_input=data/test_labels.csv  --output_path=data/test.record
```

Grab the COCO models

```
wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2017_11_17.tar.gz
```
or you can click [here](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2017_11_17.tar.gz)

Start training the model

```
python3 train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1_pets.config
```

Export graph and the model we have trained
```
python3 export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path training/ssd_mobilenet_v1_pets.config \
    --trained_checkpoint_prefix training/model.ckpt-1000 \
    --output_directory plate_model_exported
```

Start jupyter notebook inside the directory to see the results
```
jupyter notebook --notebook-dir=.
```