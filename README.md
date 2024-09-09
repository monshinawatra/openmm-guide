# openmm-guide

## Dataset
### DataLoader
You need to configure the dataloader in the config file. Here is an example of the dataloader configuration for the coco detection dataset.
```py
# Variables
dataset_type = "CocoDataset"
data_root = "data/coco/"
# MM-detection Config
train_dataloader = dict(
    batch_size=2,
    num_workers=2,
    persistent_workers=True,
    sampler=dict(type='DefaultSampler', shuffle=True),
    batch_sampler=dict(type='AspectRatioBatchSampler'),
    dataset=dict(
        type=dataset_type,
        data_root=data_root,
        ann_file="train/_annotations.coco.json",
        data_prefix=dict(img="train/")))

val_dataloader = dict(
    batch_size=1,
    num_workers=2,
    persistent_workers=True,
    drop_last=False,
    sampler=dict(type="DefaultSampler", shuffle=False),
    dataset=dict(
        type=dataset_type,
        data_root=data_root,
        ann_file="valid/_annotations.coco.json",
        data_prefix=dict(img="valid/"),
        test_mode=True))

test_dataloader = val_dataloader
```

If your class labels are not in the default format, you can configure the class labels in `metainfo` in the dataset configuration.

```py
# Variables
classes = ("car", "license",)
# MM-detection Config
train_dataloader = dict(
    dataset=dict(
        metainfo=dict(classes=classes)))

```

### Data Evaluation
Here is an example that you can set the evaluation metric in the config file.
```py
# MM-detection Config
val_evaluator = dict(
    type='CocoMetric',
    ann_file=data_root + 'annotations/instances_val2017.json',
    metric='bbox',
    format_only=False,
    backend_args=backend_args)

test_evaluator = val_evaluator
```

## Visualization
### WandB Logging

```py
vis_backends = [
    dict(type="LocalVisBackend"),
    dict(type="WandbVisBackend", init_kwargs=dict(project="Vehicle&LicensePlate")),
]
visualizer = dict(type="DetLocalVisualizer", vis_backends=vis_backends, name="visualizer")

```

## Optimizer
### Configuration
- Gradient accumulation
```py
optim_wrapper = dict(accumulative_counts=4)
```
