# PyTorch Template Project
PyTorch deep learning project made easy.

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->
<!-- /code_chunk_output -->

## Requirements
* Python >= 3.5 (3.6 recommended)
* PyTorch >= 0.4 (1.2 recommended)
* tqdm (Optional for `test.py`)
* tensorboard >= 1.14 (see [Tensorboard Visualization](#tensorboard-visualization))


## Folder Structure
  ```
  Kimchi-Classification/
  │
  ├── train.py - main script to start training
  ├── test.py - evaluation of trained model
  │
  ├── config.json - holds configuration for training
  ├── parse_config.py - class to handle config file and cli options
  │
  ├── new_project.py - initialize new project with template files
  │
  ├── base/ - abstract base classes
  │   ├── base_data_loader.py
  │   ├── base_model.py
  │   └── base_trainer.py
  │
  ├── data_loader/ - anything about data loading goes here
  │   └── data_loaders.py
  │
  ├── data/ - default directory for storing input data
  │
  ├── model/ - models, losses, and metrics
  │   ├── model.py
  │   ├── metric.py
  │   └── loss.py
  │
  ├── saved/
  │   ├── models/ - trained models are saved here
  │   └── log/ - default logdir for tensorboard and logging output
  │
  ├── trainer/ - trainers
  │   └── trainer.py
  │
  ├── logger/ - module for tensorboard visualization and logging
  │   ├── visualization.py
  │   ├── logger.py
  │   └── logger_config.json
  │  
  └── utils/ - small utility functions
      ├── util.py
      └── ...
  ```

## Train
`python train.py -c config.json`
`python train.py -r path/to/ckpt_file.pth`

## Test
You can test trained model by running `test.py` passing path to the trained checkpoint by `--resume` argument.
`python test.py -r path/to/ckpt_file.pth`

## Tensorboard Visualization
`tensorboard --logdir saved/log/`

### Config file format
Config files are in `.json` format:
```javascript
{
    "name": "Kimchi-resnet34",
    "n_gpu": 1,

    "arch": {
        "type": "resnet34",
        "args": {
        
            }
    },

    "data_loader": {
        "type": "KimchiDataLoader",
        "args":{
            "root_dir": "dataset",
            "mode": "train",
            "batch_size": 256,
            "shuffle": true,
            "validation_split": 0.0,
            "num_workers": 2
        }
    },
    
    "optimizer": {
        "type": "Adam",
        "args":{
            "lr": 0.001,
            "weight_decay": 0.0001,
            "amsgrad": true
        }
    },
    "loss": "cross_entropy_loss",
    "metrics": [
        "accuracy_"
    ],

    "lr_scheduler": {
        "type": "StepLR",
        "args": {
            "step_size": 30,
            "gamma": 0.1
        }
    },
    
    "trainer": {
        "epochs": 250,

        "save_dir": "saved/",
        "save_period": 4,
        "verbosity": 2,
        
        "monitor": "min val_loss",
        "early_stop": 60,

        "tensorboard": true
    }
}

```
**Note**: checkpoints contain:
  ```python
  {
    'arch': arch,
    'epoch': epoch,
    'state_dict': self.model.state_dict(),
    'optimizer': self.optimizer.state_dict(),
    'monitor_best': self.mnt_best,
    'config': self.config
  }
  ```
