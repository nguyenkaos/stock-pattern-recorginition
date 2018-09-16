# stock-pattern-recorginition
In conclusion, this project presents a method with deep learning for head and shoulders (HAS)
pattern recognition. This appraoce uses 2D candlestick chart as input instead of 1D vectors to
predict the stock trend. The reason for using 2D images is that images about the stock pricelike candlestick chart are more often used for stock investors and easier to understand. Compared with feeding with 1D vector, this approach almost does not need any preprocessing, and the model can feed with raw pixels. In addition, images can contain more
than one time-series. Another reason is that Convolutional Neural network (CNN) is more
stable.

This approach can help traders at all levels to analyze stock market
without much experience in the stock market. It can help investors find HAS patterns from
the stock market quickly without many human resources. 185 head and shoulders (HAS)
patterns are collected and labeled from 20 stock indexes. FR-CNN is used to train a model
with 150 of those images. As expected, with only 150 stock images, AP@0.5IOU is 64%.
There still exists the over-fitting problem in this model, because of 150 images are not
enough for training. To solve these over-fitting two methods are provided, including data
segmentation method and data variation method. Data segmentation aims to remove
noises from a simple chart. However, this method leads to a significant increase in false
positives. Data variation method is a better way to solve this problem. The model fed with
8000 variation images based on 30 original images has an AP@0.5IOU of 74% which
increased 10% compared with the model with 150 original images.

#stock pattern
Head and shoulders Head and shoulders pattern is one of the most used patterns in stock analysis, becauseofitsdistinctiveshapewhichisdevelopedbytwotrendlineswhichconverge. This pattern always exists in an increase period. Two valley makes the support line (or neckline) while ﬁrst and third peak make the resistance line. Similar to ‘M’ and ‘W’ pattern, once the price breaks through the support line, the price would decrease, traders would better to sell after breaking through the support line. On the contract, if the price breaks the resistance line, it normally would increase continually. Therefore, it is a better trade strategy to buy after the price break the resistance line.

FIGURE 3.6: Head and shoulders

Inverse Head and shoulders Inverse head and shoulders pattern is the opposite of head and shoulders pattern. This pattern always exists in a drop period. Two shoulders make the support line while ﬁrst and second peak make the resistance line (or neckline). Similarly, traders would better to sell after breaking through the support line while buying after the price break the resistance line.

FIGURE 3.7: Inverse head and shoulders

Stockchartpatternsplayanimportantroleinthestockanalysisandpredictiontechnical and can be a powerful asset for traders at any level. It is a very basic level of priceactionswhichhappenedinanytimeperiod: monthly,dailyandintraday. Even for a beginner trader, if they can recognize these patterns early, they will gain a real competitive advantage in the markets. In this part, I implemented a deep learning model to recognize the common stock patterns which are helpful for any level of stock traders. More importantly, recognizing with computers is much quicker than ﬁnding out all patterns by humans. For example, it may only need a few minutes for computers to track intraday history data of all stock indexes while it may need a group of investors to work weeks. Besides saving time, a stock pattern recognition can also help the investment companies save a lot of human resources since a computer can do much more jobs than a human in a similar time period.

Followed pictures shows some generated images of trainning data.

![some generated images](https://github.com/CharlesLoo/stock-pattern-recorginition/blob/master/results/test_result_with_generated_data/variation.jpg)

The AP@0.5IOU is:

![AP](https://github.com/CharlesLoo/stock-pattern-recorginition/blob/master/results/test_result_with_generated_data/ap.png)

And the final result are shown in follow pictures.

![Some results of pattern recognition](https://github.com/CharlesLoo/stock-pattern-recorginition/blob/master/results/test_result_with_generated_data/whole.png)
# TensorFlow Object Detection Model Training

This is a summary of [this nice readme]( https://gist.github.com/douglasrizzo/c70e186678f126f1b9005ca83d8bd2ce).

1. [Install TensorFlow](https://www.tensorflow.org/install/).

2. Download the TensorFlow [models repository](https://github.com/tensorflow/models).

## Annotating the dataset
1. Fit the time serious with bottom-up or top-down segementation algorithms. 

2. Install [labelImg](https://github.com/tzutalin/labelImg). This is a Python package, you can install via pip, but the one from GitHub is better. It saves annotations in the PASCAL VOC format.

3. Annotate your dataset using labelImg.  

4. Use [this script](https://github.com/datitran/raccoon_dataset/blob/master/xml_to_csv.py) to convert the XML files generated by labelImg into a single CSV file.

    cd get_data/
    
      python xml2csv.py 

5. Separate the CSV file into two, one with training examples and one with evaluation examples. Images should be selected randomly, making sure that objects from all classes are present in both of them. The usual proportions are 75 to 80% training and the rest to the evaluation dataset.

6. Use [this script](https://github.com/datitran/raccoon_dataset/blob/master/generate_tfrecord.py) to convert the two CSV files (eg. train.csv and eval.csv) into TFRecord files (eg. train.record and eval.record), the data format TensorFlow is most familiar with.

    get_data/
    
    $ python general_tf_record.py --csv_input=label_training/raccoon_labels.csv --output_path=label_training/train.record

## Traversing the text file hell...

1. Create a label map, like [one of these](https://github.com/tensorflow/models/tree/master/research/object_detection/data). Make sure class numbers are exactly the ones that were used when creating the TFRecords.

2. Download one of the neural network models provided in [this page](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md). The ones trained in the MSCoco dataset are the best ones, since they were also trained on objects.

3. Provide a training pipeline, which is a `config` that usually comes in the tar.gz file downloaded in the last step. If they don't, they can be found [here]( https://github.com/tensorflow/models/tree/master/research/object_detection/samples/configs) (they need some tweaking before using, for example, changing number of classes). A tutorial on how to create your own [here](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/configuring_jobs.md).

 * The pipeline config file has some fields that must be adjusted before training is started. Its header describes which ones. Usually, they are the fields that point to the label map, the training and evaluation directories and the neural network checkpoint. In case you downloaded one of the models provided in [this page](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md), you should untar the `tar.gz` file and point the checkpoint path inside the pipeline config file to the "untarred" directory of the model (see [this answer](https://stackoverflow.com/a/45363576/1245214) for help).

 * You should also check the number of classes. MSCoco has 90 classes, but your problem may have more or less.

## Training the network

1. Train the model. [This is how you do it locally](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/running_locally.md). **Optional:** in order to check training progress, TensorBoard can be started pointing its `--logdir`  to the `--train_dir` of object_detection/train.py.

2. Export the network, like [this](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/exporting_models.md).

3. Use the exported `.pb` in your object detector.

## Tips

In the _data augmentation_ section of the training pipeline, some options can be added or removed to try and make the training better. Some of the options are listed [here](https://stackoverflow.com/a/46901051)

1. If you are running out of memory and this is causing training to fail, there are a number of solutions you can try. First try adding  the arguments

      batch_queue_capacity: 2
      
      prefetch_queue_capacity: 2
  
  to your config file in the train_config section. For example, placing the two lines between gradient_clipping_by_norm and fine_tune_checkpoint will work. The number 2 above should only be starting values to get training to begin. The default for those values are 8 and 10 respectively and increasing those values should help speed up training.
