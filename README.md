About
=====

Implementation BEGAN([Boundary Equilibrium Generative Adversarial Networks](https://arxiv.org/pdf/1703.10717.pdf)) by Keras.

Version
-------
Developed by these software versions.

* Mac OS Sierra: 10.12.4
* Python: 3.5.3
* Keras: 2.0.3
* Theano: 0.9.0
* Pillow: 4.1.0


How to Use
=======

Setup
--------

```bash
pip install -r requirements.txt
```

Create Dataset
------------

### Prepare Image
You can use any square images.
For example, 

images in [http://vis-www.cs.umass.edu/lfw/](http://vis-www.cs.umass.edu/lfw/)  

```text
[new] All images aligned with deep funneling 
(111MB, md5sum 68331da3eb755a505a502b5aacb3c201)
```

### Convert Images to 64x64 pixels

#### Install imagemagick
For `convert` command, install imagemagick.

```bash
brew install imagemagick
```

#### Convert Images
* `ORIGINAL_IMAGE_DIR`: dir of original JPG images
* `TARGET_DIR`: dir of after converted images

```bash
ORIGINAL_IMAGE_DIR=PATH/TO/ORIGINAL/IMAGE_DIR
CONVERTED_DIR=PATH/TO/CONVERTED/IMAGE_DIR

mkdir -p "$CONVERTED_DIR"
for f in $(find "$ORIGINAL_IMAGE_DIR" -name '*.jpg')
do
  echo "$f"
  convert "$f" -resize 64x64 ${CONVERTED_DIR}/$(basename $f)
done
```

#### Create Dataset

```bash
PYTHONPATH=src python src/began/create_dataset.py "$CONVERTED_DIR"
```

Training BEGAN
--------------

```bash
PYTHONPATH=src python src/began/training.py
```

Images are generated in each epoch into `generated/epXXX/` directory.

### Log Example
```txt
2017-04-14 11:38:20.964583: (13194, 64, 64, 3)
2017-04-14 11:38:23.032517: LearningRate=0.0001000
2017-04-14 11:49:54.768748: ep=1, b_idx=823/823, MGlobal=0.14640, Loss(D)=0.09456, Loss(G)=0.01808, Loss(X)=0.09497, Loss(G(Zd))=0.01654, K=0.025006
((20, 64, 64, 3), 0.058249116, 0.73761588)
2017-04-14 11:49:54.987647: LearningRate=0.0001000
2017-04-14 12:01:03.759311: ep=2, b_idx=823/823, MGlobal=0.11076, Loss(D)=0.08506, Loss(G)=0.03930, Loss(X)=0.08665, Loss(G(Zd))=0.04003, K=0.039732
((20, 64, 64, 3), -0.01763171, 1.0418562)
2017-04-14 12:01:03.995350: LearningRate=0.0001000
2017-04-14 12:12:12.259534: ep=3, b_idx=823/823, MGlobal=0.09524, Loss(D)=0.08278, Loss(G)=0.05535, Loss(X)=0.08483, Loss(G(Zd))=0.04529, K=0.045397
((20, 64, 64, 3), -0.030694725, 1.0436766)
...
```

### FYI: epoch time in training

About 680 sec/epoch

* Linux
* Dataset: `All images aligned with deep funneling` (13194 samples)
* Intel(R) Core(TM) i7-7700K CPU @ 4.20GHz
* GeForce GTX 1080
* Environment Variables

  ```bash
  KERAS_BACKEND=theano
  THEANO_FLAGS=device=gpu,floatX=float32,lib.cnmem=1.0
  ```

Generate Image
---------------

```bash
PYTHONPATH=src python src/began/generate_image.py
```

Generated images are outputted in `generated/main/` directory.

### Generated Image Examples

<table>
<tr>
  <th>Epoch 1</th>
  <th><img src="example/ep001/gen_000.jpg"></th>
  <th><img src="example/ep001/gen_001.jpg"></th>
  <th><img src="example/ep001/gen_002.jpg"></th>
  <th><img src="example/ep001/gen_003.jpg"></th>
  <th><img src="example/ep001/gen_004.jpg"></th>
</tr>
<tr>
  <th>Epoch 25</th>
  <th><img src="example/ep025/gen_000.jpg"></th>
  <th><img src="example/ep025/gen_001.jpg"></th>
  <th><img src="example/ep025/gen_002.jpg"></th>
  <th><img src="example/ep025/gen_003.jpg"></th>
  <th><img src="example/ep025/gen_004.jpg"></th>
</tr>
</table>


Memo
========

Trouble: Running with Theano 0.9.0 CPU mode on Linux
-------------
Theano 0.9.0 CPU mode on Linux seems to have memory leak problem.
See: https://github.com/fchollet/keras/issues/5935



