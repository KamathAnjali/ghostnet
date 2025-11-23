## ghostnet

implementation of the paper [ghostnet paper](https://arxiv.org/abs/1911.11907) 

### running locally

- start a virtual env with `python3 -m venv .` 
- run `python ghostnet.py` 

### experiments

#### ghostnet-resnet-56

| no.   | ratio(s) | kernel(d) | optimizer | lr_scheduler          | accuracy(ours) | accuracy(paper) | epochs | file          | comment |
| ----- | -----    | -----     | -----     | -----                 | -----          | -----           | -----  | ----          | -----   |
| 1     | 2        | 3         | SGD       | CosineAnnealing       | 92.20%        | 92.7%           | 200     | gr_cosine.pth | Had smooth convergence|
| 2     | 2        | 3         | SGD       | MultiStepLR(100, 500) | 92.55%        | 92.7%           | 200     | gr_multistep.pth | Had smooth convergence|

- total feature maps = s * number of intrinsic feature maps (produced by ordinary convolution filters)
- `images/ghost_visualization.png` (dog's image, cifar index=12) proves that ghost map preserves the exact same shape and pose as the intrinsic maps
- the ghost versions appear slightly shifted and smoothened. this confirms that the cheap operation (depthwise convolutions) learned useful transformations like blurring and edge enhancement to augment the feature space without the computationally heavy full convolution operation.
- computational reduction(measured using `thops`):
```
measuring standard resnet-56...
  flops: 127.93M
  params: 0.86M

measuring ghost-resnet-56...
  flops: 67.50M
  params: 0.44M

results:
  flops reduction: 1.90x (paper claims ~2x)
  param reduction: 1.95x (paper claims ~2x)
```
