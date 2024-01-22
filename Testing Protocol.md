
Data copying algorithm speed, data duplication and transfer process. 
Data access issues, cold storage or fast cache.

Docker container natively, cnvrg is wrapped with their cli and api. 

```
docker run
#should just work
```

email idea, on how the protocol works

persistent workspaces

### Example Protocol
---
##### MLOPS Test Protocol: Persistent Workspace
	assumptions:  native setup MLOPs flavor, can be run out of a registry, 
	test:         persistent global file spaces, active environment, sessions, terminal stdout, kernel attatch

*(in a general Kubernetes environment)*

1. use registry `nvcr.io/torch:23-08-py`
	1. (pick a standard open source image that we always grab, some need to be changed before they are used/ or default container)
	2. `ngc` / `cnvrg5.0`
	3. ngc might not run natively
	4. prerun that `pip install`
2. start a workspace, name of MLOPs test, 
	1. `small gpu`
	2. `pytorch:23-08-py3`
	3. preserved in `/cnvrg`, unless use volume
3. `env` click out, click back in, check if terminal output persists, 
4. open up different browsers, check it shows up on different web sessions 

1. use registry `nvcr.io/torch:23-08-py`
2. spin up container: gpu, persistent volume, jupyter interactive session, terminal
3. std out persistent test across web sessions
	1. open terminal
	2. env 
	3. switch browser
	4. open new browser
	5. navigate back to workspace terminal
		1. test terminal window close and reattach to terminal session
4. python kernel persistent test across web sessions:
	1. opened a jupyter notebook
	2. run some cells
		1. commands
	3. open new browser
	4. navigate back to jupyter notebook
	5. check for variable declarations


##### MLOPs Test: Batch submissions
*cnvrg experiments*

1. Basic test
	1. run job `env > text.txt`
	2. read file, check if contains environment
2. Training test
	1. create a workspace (volume if needed)
		1. `mkdir tensorflow`
		2. `cd tensorflow`
		4. create tensorflow example
			1. https://github.com/tensorflow/datasets/blob/master/docs/keras_example.ipynb
		5. create wrapper bash file that pip installs and runs the file. 
			`#!/bin/bash`
			`pip install tensorflow tensorflow_datasets`
			`python3 tensorflow/main.py`
	1. run the batch job
		1. run the wrapper bash file
		2. use docker image tensorflow:23.01-tf2-py3 

##### MLOPs Test: Email
1. Run Basic Batch Job
	1. `env > text.txt`
2. Receive email
##### MLOPs Test: Track Experiment

1. Training Test
	1. create a workspace (volume if needed)
		1. `mkdir tensorflow`
		2. `cd tensorflow`
		3. create tensorflow example
			1. https://github.com/tensorflow/datasets/blob/master/docs/keras_example.ipynb
			2. `wget https://raw.githubusercontent.com/tensorflow/datasets/master/docs/keras_example.ipynb`
			3. `jupyter nbconvert --to python keras_example.ipynb --output "main"`
			4. if needed, add in a callback so that training prints logs that are recognized by the graphing mechanism
		4. create wrapper bash file that pip installs and runs the file, ensuring dependencies
			`#!/bin/bash`
			`pip install tensorflow tensorflow_datasets`
			`python3 tensorflow/main.py`
	1. run the batch job
		1. run the wrapper bash file
		2. use docker image tensorflow:23.01-tf2-py3 
	2. view graphs

Example callback
```
class MyCallback(tf.keras.callbacks.Callback):
    def on_batch_end(self, batch, logs=None):
        print("cnvrg_linechart_mnistloss value:", logs["loss"])

model.fit(
    ds_train,
    epochs=6,
    validation_data=ds_test,
    callbacks=[MyCallback()]
)
```

Bash script that automates all of step 1.
```bash
#!/bin/bash
wget https://raw.githubusercontent.com/tensorflow/datasets/master/docs/keras_example.ipynb
jupyter nbconvert --to python keras_example.ipynb --output "main"
sed -i '/model.fit/i class MyCallback(tf.keras.callbacks.Callback):\
    def on_batch_end(self, batch, logs=None):\
        print("cnvrg_linechart_mnistloss value:", logs["loss"])' main.py
sed -i 's/\(validation_data=ds_test\)/\1,\
    callbacks=[MyCallback()]/' main.py
echo '#!/bin/bash\
pip install tensorflow tensorflow_datasets\
python3 tensorflow/main.py' > run.sh
```
##### MLOPs Test: Registry Viewer

1. Containers > Add Registry
2. Containers > Add Image
3. Create Workspace
	1. New Workspace
	2. Images (check if image is available)


##### MLOPs Test: Dataset
Prerequisites: MLOPs Testing: CLI

*cnvrg seems to not have a direct way to load from cdn to dataset
1. On a different machine with a cli installed, download mnist (with wget)
2. upload it to the MLOPs platform

```
```bash

mkdir mnist_download
cd mnist_download
echo "Downloading..."

wget --no-check-certificate http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz
wget --no-check-certificate http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz
wget --no-check-certificate http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz
wget --no-check-certificate http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz

echo "Unzipping..."

gzip -f -d train-images-idx3-ubyte.gz
gzip -f -d train-labels-idx1-ubyte.gz
gzip -f -d t10k-images-idx3-ubyte.gz
gzip -f -d t10k-labels-idx1-ubyte.gz

cd ..
echo "done"
echo "uploading to cnvrg"

cnvrgv2 dataset put -n mednist -f train-images-idx3-ubyte,train-labels-idx1-ubyte,t10k-images-idx3-ubyte,t10k-labels-idx1-ubyte -or
"
```

##### MLOPs Test: Pipeline/ Workflow 
Prerequisites: 
MLOPs Test: Tracking
MLOPs Test: Dataset

*flow without dataset
1. Follow MLOPs Test:Tracking to set up a workspace
2. Flows > new task > custom task > enter command
	1. pip install tensorflow tensorflow_datasets idx2numpy numpy
	2. python3 /cnvrg/tensorflow/main.py

*train with dataset
1. Follow MLOPs Test: Dataset to setup a dataset
2. Follow MLOPs Test: Tracking to setup a workspace
3. save below python file as run.py
4. Flows > new task > data task > mednist
5. Flows > new task > custom task > enter command
	1. pip install tensorflow tensorflow_datasets idx2numpy numpy
	2. python3 /cnvrg/tensorflow/run.py
	
```
import tensorflow as tf
import numpy as np
import idx2numpy

train_x = idx2numpy.convert_from_file('/data/mednist/train-images-idx3-ubyte')
train_y = idx2numpy.convert_from_file('/data/mednist/train-labels-idx1-ubyte')
test_x = idx2numpy.convert_from_file('/data/mednist/t10k-images-idx3-ubyte')
test_y = idx2numpy.convert_from_file('/data/mednist/t10k-labels-idx1-ubyte')

def create_dataset(x_tr, x_ts, y_tr, y_ts, batch_size):
    AUTOTUNE = tf.data.experimental.AUTOTUNE

    train_dataset = tf.data.Dataset.from_tensor_slices((x_tr, y_tr))
    train_dataset = train_dataset.shuffle(buffer_size=1000, reshuffle_each_iteration=True)
    train_dataset = train_dataset.batch(batch_size).prefetch(AUTOTUNE)

    test_dataset = tf.data.Dataset.from_tensor_slices((x_ts, y_ts))
    test_dataset = test_dataset.batch(batch_size).prefetch(AUTOTUNE)

    return train_dataset, test_dataset

train, test = create_dataset(train_x, test_x, train_y, test_y)

model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(28, 28)),
  tf.keras.layers.Dense(128, activation='relu'),
  tf.keras.layers.Dense(10)
])

model.compile(
    optimizer=tf.keras.optimizers.Adam(0.001),
    loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
    metrics=[tf.keras.metrics.SparseCategoricalAccuracy()],
)

class MyCallback(tf.keras.callbacks.Callback):
    def on_batch_end(self, batch, logs=None):
        print("cnvrg_linechart_mnistloss value:", logs["loss"])

model.fit(
    train,
    epochs=6,
    validation_data=test,
    callbacks=[MyCallback()]
)

```
