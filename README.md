# SAR Processing

Repository for ODS Summer of Code event with preprocessing and segmentation pipelines.

Preprocessing pipeline uses _GDAL_ and _OpenCV_ libraries. Segmentation pipeline uses _PyTorch_ and _Segmentation Models_ frameworks.

It is supposed to be used with _Google Colaboratory_ (_File -> Open notebook -> GitHub_).


## Setup Kaggle Kernel for training models

[Kaggle Kernels](https://www.kaggle.com/) – is a cloud-based development environment similar to Google Colab. Kaggle provides free access to the GPU for 37 hours per week.

1. Download and upload notebook

   ``` bash
   wget https://raw.githubusercontent.com/MaritimeAI/ODS-SoC/master/segmentation.kaggle.ipynb
   ```
   Open *[Kaggle](https://www.kaggle.com/) → Create → New Notebook → File → Upload Notebook → segmentation.kaggle.ipynb*

2. Add dataset

   *File → Add or upload data*

3. Use Kaggle secrets to store your API key [wandb](wandb.ai) for experiment tracking and models managment

   *Add-ons → Secrets → Add a new secrets → Label ('wandb') → Value ([Key](https://wandb.ai/authorize))*

4. Configuration settings

   ``` Python
   config = {
       'classes': ['nodata', 'water', 'ice'],
       'batch_size_train': 1,
       'batch_size_valid': 1,
       'num_workers_train': 1,
       'num_workers_valid': 1,
       'model_encoder': 'ResNet32',
       'model_pretrain': 'ImageNet',
       'model_channels': 3,
       'data_split': 1,
       'expand': True,
       'debug': True,
       'flat': True,
   }
   ```
   - data_split - cross-validation for 5 folds or comment this line
   - debug - test run for one image
   - model_encoder - choose any from list [encoders](https://github.com/qubvel/segmentation_models.pytorch#encoders-)

5. Choose architecture model

   *Notebook → section __Model__*
   ```Python
   model = smp.MAnet(encoder_name=config['model_encoder'].lower(),
                     encoder_weights=config['model_pretrain'].lower(),
                     in_channels=config['model_channels'],
                     classes=len(CLASSES))
   ```
6. Save weights model

   *Save version → Advenced settings → Always save output → Quick save → Save*

7. Upload weights and trained model

   *File → Add or upload data → Notebook Output Files → Your work → Chosse previos version*

   In section __Paths__ copy notebook name to save the weights to working directory

   ``` Python
   NOTEBOOK_NAME = ''
   ```
   In section __Train loop__ add last saved model

   ```Python
   NAME_PRELOAD = ''
   ```


