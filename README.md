Improved Natural Language Generation via Loss Truncation
========================================================

This is the official repository for Improved Natural Language Generation via Loss Truncation.

We provide code for loss dropping.


What is loss truncation?
========================


Installation
============
We require Python 3.5+ and torch 1.0+.

To install `loss_dropper`, in a virtual environment of your choice, run:
```
pip install -U git+https://github.com/ddkang/loss_dropper.git
```


Usage
=====
1. Import loss_dropper:
```
from loss_dropper import LossDropper
```

2. Initialize `LossDropper`:
```
self.dropper = LossDropper(dropc=dropc)
```

3. Initialize your loss:
```
self.criterion = nn.NLLLoss(weight, reduction='none')
```
*IMPORTANT*: loss truncation performs dropping at the _sequence_ level. The standard reduction in pytorch will aggregate over the wrong dimensions for truncation.

4. Do loss dropping:
```
loss = loss.view(-1, batch_size)  # view by batch size
loss = loss.mean(dim=0)  # aggregate by sequence
mask = self.dropper(loss)  # The dropper returns a mask of 0s where data should be dropped.
loss *= mask  # Mask out the high losses
loss = loss.mean()  # Aggregate
```
*IMPORTANT*: depending on how your loss functions, you may have to aggregate in different ways.


Citation
========

If you find this useful in your research, please consider citing:
```
@article{kang2020improved,
  title={Improved Natural Language Generation via Loss Truncation},
  author={Daniel Kang and Tatsunori Hashimoto},
  journal={ACL},
  year={2020}
}
```
