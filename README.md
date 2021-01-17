# Speech Emotion Recognition Example
Fork From Below Repo 

**Modulated Fusion using Transformer for Linguistic-Acoustic Emotion Recognition**  
[[paper]](https://arxiv.org/abs/2010.02057) [[repo]](https://github.com/jbdel/modulated_fusion_transformer)

```
@inproceedings{delbrouck-etal-2020-modulated,
    title = "Modulated Fusion using Transformer for Linguistic-Acoustic Emotion Recognition",
    author = "Delbrouck, Jean-Benoit  and
      Tits, No{\'e}  and
      Dupont, St{\'e}phane",
    booktitle = "Proceedings of the First International Workshop on Natural Language Processing Beyond Text",
    month = nov,
    year = "2020",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/2020.nlpbt-1.1",
    doi = "10.18653/v1/2020.nlpbt-1.1",
    pages = "1--10",
    abstract = "This paper aims to bring a new lightweight yet powerful solution for the task of Emotion Recognition and Sentiment Analysis. Our motivation is to propose two architectures based on Transformers and modulation that combine the linguistic and acoustic inputs from a wide range of datasets to challenge, and sometimes surpass, the state-of-the-art in the field. To demonstrate the efficiency of our models, we carefully evaluate their performances on the IEMOCAP, MOSI, MOSEI and MELD dataset. The experiments can be directly replicated and the code is fully open for future researches.",
}
```

#### Environement

This repo fixed version issue in original repo

```
python 3.8
pytorch 1.7
torchvision 0.8
spacy 2.3
```
Use version 2.3 for spacy v2.3
```
wget https://github.com/explosion/spacy-models/releases/download/en_vectors_web_lg-2.3.0/en_vectors_web_lg-2.3.0.tar.gz -O en_vectors_web_lg-2.3.0.tar.gz --no-check-certificate     
pip install en_vectors_web_lg-2.3.0.tar.gz
```

- Original Repo  
   Create a 3.6 python environement with:
   ```
   torch              1.2.0    
   torchvision        0.4.0   
   numpy              1.18.1    
   ```
    
   We use GloVe vectors from space. This can be installed to your environement using the following commands :
   ```
   wget https://github.com/explosion/spacy-models/releases/download/en_vectors_web_lg-2.1.0/en_vectors_web_lg-2.1.0.tar.gz -O en_vectors_web_lg-2.1.0.tar.gz
   pip install en_vectors_web_lg-2.1.0.tar.gz
   ```

#### Data

Create a data folder and get the data:
```
mkdir -p data
cd data
wget -O data.zip https://www.dropbox.com/s/tz25q3xxfraw2r3/data.zip?dl=1
unzip data.zip
```

#### Training

Here is an example to train a MAT model on IEMOCAP:

```
mkdir -p ckpt
for i in {1..10}
do
    python main.py --dataset IEMOCAP \
                   --model Model_MAT \
                   --multi_head 4 \
                   --ff_size 1024 \
                   --hidden_size 512 \
                   --layer 2 \
                   --batch_size 32 \
                   --lr_base 0.0001 \
                   --dropout_r 0.1 \
                   --dropout_o 0.5 \
                   --name mymodel
done

```
Checkpoints will be stored in folder `ckpt/mymodel`

#### Evaluation 

You can evaluate a model by typing : 
```
python ensembling.py --name mymodel --sets test
```
The task settings are defined in the checkpoint state dict, so the evaluation will be carried on the dataset you trained your model on.

By default, the script globs all the training checkpoints inside the folder and ensembling will be performed
To show further details of the evaluation from a specific ensembling, you can use the `--index` argument:
```
python ensembling.py --name mymodel --sets test --index 5
```

#### Pre-trained model  
We release pre-trained models to replicate the results as shown in the paper. Models should be placed in the `ckpt` folder.
```
mkdir -p ckpt
```

[IEMOCAP 4-class emotions](https://www.dropbox.com/s/wzoiwrtc9m3nb78/IEMOCAP_pretrained.zip?dl=1)
```
python ensembling.py --name IEMOCAP_pretrained --index 5 --sets test

              precision    recall  f1-score   support

           0       0.70      0.66      0.68       384
           1       0.68      0.75      0.71       278
           2       0.79      0.71      0.75       194
           3       0.78      0.81      0.79       229

    accuracy                           0.73      1085
   macro avg       0.74      0.73      0.73      1085
weighted avg       0.73      0.73      0.73      1085

Max ensemble w-accuracies for test : 72.53456221198157
```

[MOSEI 2-class sentiment](https://www.dropbox.com/s/t2p8soswt9t1ii4/MOSEI_pretrained.zip?dl=1)
```
python ensembling.py --name MOSEI_pretrained --index 9 --sets test

              precision    recall  f1-score   support

           0       0.75      0.57      0.65      1350
           1       0.84      0.92      0.88      3312

    accuracy                           0.82      4662
   macro avg       0.80      0.75      0.77      4662
weighted avg       0.82      0.82      0.81      4662

Max ensemble w-accuracies for test : 82.15358215358215
```

[MOSI 2-class sentiment](https://www.dropbox.com/s/zw4a9ukk1npzt9r/MOSI_pretrained.zip?dl=1)
```
python ensembling.py --name MOSI_pretrained --index 2 --sets test


              precision    recall  f1-score   support

           0       0.77      0.91      0.84       379
           1       0.84      0.63      0.72       277

    accuracy                           0.79       656
   macro avg       0.81      0.77      0.78       656
weighted avg       0.80      0.79      0.79       656

Max ensemble w-accuracies for test : 79.26829268292683
```
[MELD 7-class emotions](https://www.dropbox.com/s/458h1ze6cic3h1l/MELD_pretrained.zip?dl=1)
```
python ensembling.py --name MELD_pretrained --index 9 --sets test


              precision    recall  f1-score   support

           0       0.64      0.52      0.58      1256
           1       0.36      0.58      0.45       281
           2       0.08      0.18      0.11        50
           3       0.23      0.25      0.24       208
           4       0.44      0.47      0.46       402
           5       0.23      0.24      0.23        68
           6       0.31      0.27      0.29       345

    accuracy                           0.45      2610
   macro avg       0.33      0.36      0.34      2610
weighted avg       0.48      0.45      0.46      2610
```

