# BERT-NER

Use Google's BERT for named entity recognition（CoNLL-2003 as the dataset.

### Folder Description:
```
BERT-NER
|____ bert
|____ cased_L-12_H-768_A-12	    # need download from [here](https://storage.googleapis.com/bert_models/2018_10_18/cased_L-12_H-768_A-12.zip)
|____ data		            # train data
|____ middle_data	            # middle data (label id map)
|____ output			    # output (final model, predict results)
|____ BERT_NER.py		    # main code
|____ conlleval.pl		    # eval code
|____ run_ner.sh    		    # run model and eval result

```

### Steps:
1. Clone this repo:
2. ``cd bert``
3. ``git clone https://github.com/google-research/bert .``
4. Download [cased_L-12_H-768_A-12](https://storage.googleapis.com/bert_models/2018_10_18/cased_L-12_H-768_A-12.zip) model and extract it's contents to directory ``cased_L-12_H-768_A-12``.
5. Install ``tensorflow`` or ``tensorflow-gpu``. Tested on v1.13.1 and python 3.6.8.
6. From the root directory, run: ``bash run_ner.sh``

### What's in run_ner.sh:
```
python BERT_NER.py\
    --task_name="NER"  \
    --do_lower_case=False \
    --crf=False \
    --do_train=True   \
    --do_eval=True   \
    --do_predict=True \
    --data_dir=data   \
    --vocab_file=cased_L-12_H-768_A-12/vocab.txt  \
    --bert_config_file=cased_L-12_H-768_A-12/bert_config.json \
    --init_checkpoint=cased_L-12_H-768_A-12/bert_model.ckpt   \
    --max_seq_length=128   \
    --train_batch_size=32   \
    --learning_rate=2e-5   \
    --num_train_epochs=3.0   \
    --output_dir=./output/result_dir

perl conlleval.pl -d '\t' < ./output/result_dir/label_test.txt
```

**Notice:** According to [this](https://arxiv.org/abs/1810.04805) paper, the cased model was recommended. CoNLL-2003 dataset and perl Script comes from [here](https://www.clips.uantwerpen.be/conll2003/ner/)

### RESULTS:(On test set)
#### Parameter setting:
* do_lower_case=False
* num_train_epochs=4.0
* crf=False

```
accuracy:  98.15%; precision:  90.61%; recall:  88.85%; FB1:  89.72
              LOC: precision:  91.93%; recall:  91.79%; FB1:  91.86  1387
             MISC: precision:  83.83%; recall:  78.43%; FB1:  81.04  668
              ORG: precision:  87.83%; recall:  85.18%; FB1:  86.48  1191
              PER: precision:  95.19%; recall:  94.83%; FB1:  95.01  1311
```

### reference:

[1] https://arxiv.org/abs/1810.04805
[2] https://github.com/google-research/bert
