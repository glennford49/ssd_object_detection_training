# ssd_training

1. gather data for costum dataset to a folder named images
2. hand label dataset using labelImg(need to be installed), will be converted to .xml files  (saved to folder named annotations for training) 
3. copy 30% of your .xml files from annotations folder to a new folder, mine is named annotations_2 for testing

4. convert .xml files to .csv files using script xml_to_csv.py  (create train_labels.csv and test_labels.csv) see usage
5  in generate_tfrecord.py find class_text_to_int(row_label) change it depending on your class-names, 
           create train.record and test.record ,see usage
4. in pascal_label_map.pbtxt , just add item if your class or object to be detected is greater than 3 
5. in ssd_mobilenet_v1_pets.config edit :
        num_classes: 3  # depending on your class/item 
        batch_size:   # depends on what you want
        fine_tune_checkpoint:  # path of your model.ckpt

        train_input_reader:
            input_path:    # path of your train.record file
            label_map_path: # path of your pascal_label_map.pbtxt
            num_examples: 800   # edit it
        eval_input_reader:
            input_path:    # path of your test.record file
            label_map_path: # path of your pascal_label_map.pbtxt


        TRAINING !!!!

6. run python train.py --logtostderr --pipeline_config_path=/home/glenn/Documents/train_ssd/my_train_ssd/train_config/ssd_mobilenet_v1_pets.config --train_dir=train_logs 2>&1 | tee logs/train_$now.txt &
  
 it will take some time,
 creates 4 files :
  - model.ckpt-****.meta
  - model.ckpt-****.index
  - model.ckpt-****.data-00000-of-00001
  - pipeline.config

CONVERTING 3 FILES ABOVE to .pb
  open the script export_inference_graph.py , see usage and run it on terminal

The expected output would be in the directory
path/to/exported_model_directory (which is created if it does not exist)
with contents:
 - inference_graph.pbtxt
 - model.ckpt.data-00000-of-00001
 - model.ckpt.info
 - model.ckpt.meta
 - frozen_inference_graph.pb
 + saved_model (a directory)


ENJOY....!!!!!




