# ECG Myocardial Infarction Classifier, IID baseline

End to end pipeline to classify ECG page images into four classes, Normal, MI, PMI, HB, using ResNet18.
This repository includes data preprocessing, IID balanced splits, visualization utilities, training, evaluation, and a simple Gradio app for inference.

## Repository layout

```
.
├── app/
│   └── gradio_app.py
├── docs/
│   └── manuscript.pdf
├── src/
│   ├── preprocess_iid.py
│   ├── visualize_patterns.py
│   ├── train_resnet_iid.py
│   ├── evaluate.py
│   └── infer.py
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

Data folders like `data_iid` are ignored by git. Create them locally after preprocessing.

## Setup

```
python -m venv .venv
source .venv/bin/activate  # Windows, .venv\Scripts\activate
pip install -r requirements.txt
```

## 1, Preprocess and split, IID

```
python src/preprocess_iid.py   --raw_root /path/to/unzipped_dataset_root   --out_root data_iid   --target_size 224 224
```

## 2, Visual sanity checks

```
python src/visualize_patterns.py --data_root data_iid
```

## 3, Train ResNet18 on IID

```
python src/train_resnet_iid.py   --data_root data_iid   --epochs 15   --batch_size 32   --lr 1e-4   --weight_decay 1e-4   --out_dir checkpoints
```

## 4, Evaluate on test set

```
python src/evaluate.py   --data_root data_iid   --checkpoint checkpoints/resnet18_ecg_iid_best.pth
```

## 5, Inference and Gradio app

```
python src/infer.py   --checkpoint checkpoints/resnet18_ecg_iid_best.pth   --image /path/to/some_ecg.jpg
```

```
python app/gradio_app.py --checkpoint checkpoints/resnet18_ecg_iid_best.pth
```

