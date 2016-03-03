##NB: this is my personal hack, for the official code refer here: https://github.com/emansim/unsupervised-videos


### Getting Started


1. Compile cudamat in /cudamat:
	
	```
	make
	```
	
	If error, verify `CUDA_ROOT` in `cudamat/Makefile` to be correct.

2. Install necessary Python packages:

	* h5py (HDF5 (>= 1.8.11))
	* google.protobuf (Protocol Buffers (>= 2.5.0))
	* numpy
	* matplotlib

3. Install the Proto compiler:
	
	```
	sudo apt-get install libprotobuf-dev
	```
	
	Next compile .proto file by calling
	
	```
	protoc -I=./ --python_out=./ config.proto
	```

4. Download the datafiles into /datasets

	```
	wget http://www.cs.toronto.edu/~emansim/datasets/mnist.h5
	wget http://www.cs.toronto.edu/~emansim/datasets/bouncing_mnist_test.npy
	```
	
## Notes for training

`models/lstm_combo_1layer_mnist.pbtxt` contains the hyperparameters for length of training sequence.

Change model parameter path in  `models/lstm_combo_1layer_mnist_pretrained.pbtxt` and use instead of `models/lstm_combo_1layer_mnist.pbtxt` to continue training or obtain score on 4 frame prediction with 10 frame prediction trained model.

### Bouncing (Moving) MNIST dataset

To train a sample model on this dataset you need to set correct `data_file` in `datasets/bouncing_mnist_valid.pbtxt` and then run (you may need to change the board id of gpu): 

```
python lstm_combo.py models/lstm_combo_1layer_mnist.pbtxt datasets/bouncing_mnist.pbtxt datasets/bouncing_mnist_valid.pbtxt 0
```

After training the model and setting correct path to trained weights in `models/lstm_combo_1layer_mnist_pretrained.pbtxt`, you can visualize the sample reconstruction and future prediction results of the pretrained model by running:

```
python display_results.py models/lstm_combo_1layer_mnist_pretrained.pbtxt datasets/bouncing_mnist_valid.pbtxt 1
```

Below are the sample results, where first image is reference image and second image is prediction of the model. Note that first ten frames are reconstructions, whereas the last ten frames are future predictions.

![original](imgs/mnist_1layer_example_original.png)
![recon](imgs/mnist_1layer_example_recon.png)


### Reference

If you found this code or the paper useful, please consider citing the following paper:

```
@inproceedings{srivastava15_unsup_video,
  author    = {Nitish Srivastava and Elman Mansimov and Ruslan Salakhutdinov},
  title     = {Unsupervised Learning of Video Representations using {LSTM}s},
  booktitle = {ICML},
  year      = {2015}
}
```