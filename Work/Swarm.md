# Sam training with Swarm
This document describes where the codebase is and how to run it.

### Firmware
NVIDIA-SMI 535.86.10
Driver Version: 535.86.10
CUDA Version: 12.2
NVIDIA-CTK: 1.13.5
Docker version 24.0.5, build ced0996

----

### Dataset
https://figshare.com/articles/dataset/brain_tumor_dataset/1512427
### Docker Images

##### Make sure docker can run as non root
1. Create the `docker` group.
    
    ```
    $ sudo groupadd docker
    ```
    
2. Add your user to the `docker` group.
    
    ```
    $ sudo usermod -aG docker $USER
    ```
    
3. Log out and log back in so that your group membership is re-evaluated.
    
    > If you’re running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
    
    You can also run the following command to activate the changes to groups:
    
    ```
    $ newgrp docker
    ```
    
4. Verify that you can run `docker` commands without `sudo`.
    
    ```
    $ docker run hello-world
    ```
    
    This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.


##### Load docker image
```
docker load < sam-swarm.tar
```
\*many things may be named mnist but refer to segment anything, more refactoring needs to be done 

----
## Segment Anything
**cnvrg** environment
**project**: segmentanythingtest
**volume**: sam-workspace-1, 300 GB

*this is adapted from the segment anything github page*
https://github.com/facebookresearch/segment-anything

##### Customized files
cnvrg
	segment-anything
		notebooks
			predictor_example.ipynb
		fine-tuning.ipynb
		loadimages.py
		test_accuracy.py
	swarm
		splprisn01
			README.md
		splprisn02
			README.md

##### predictor_example.ipynb
This loads in a pretrained model as well as the dataset, specified in cell [4]
change ```
```
sam_checkpoint = "/cnvrg/segment-anything/checkpoint/sam_vit_h_4b8939.pth"
model_type = "vit_h"
```
to switch model


##### fine-tuning.ipynb
This file is an example of fine tuning, features custom diceloss function, and a decoder training segment. 
```python
	with torch.no_grad():
                image_embedding = sam.image_encoder(input_image)
                sparse_embeddings, dense_embeddings = sam.prompt_encoder(
			points=centroid_torch,
			boxes=None,
			masks=None,
		)
	low_res_masks, iou_predictions = sam.mask_decoder(
		image_embeddings=image_embedding,
		image_pe=sam.prompt_encoder.get_dense_pe(),
		sparse_prompt_embeddings=sparse_embeddings,
		dense_prompt_embeddings=dense_embeddings,
		multimask_output=False,
	)
	upscaled_masks = sam.postprocess_masks(low_res_masks, input_size, original_image_size).to(device)
	binary_mask = normalize(threshold(upscaled_masks, 0.00, 0.0))
	gt_mask_resized = torch.from_numpy(np.resize(ground_truth_masks[k], (1, 1, ground_truth_masks[k].shape[0], ground_truth_masks[k].shape[1]))).to(device)
	gt_binary_mask = torch.as_tensor(gt_mask_resized > 0, dtype=torch.float32)
	loss = loss_fn(binary_mask, gt_binary_mask)
	# return binary_mask, gt_binary_mask
	optimizer.zero_grad()
	loss.backward()
	optimizer.step()
	epoch_losses.append(loss.item())
```
generates embeddings
decoder transforms embeddings into masks
loss calculated with these masks and ground truth
uncomment return statement to view image/ masks for one epoch

##### load-images.py
This file is used by all other custom files to load in .mat images from the dataset 
bb-box is the bounding box prompt for SAM 
	uses extremes of vertices of the label
centroid is the point prompt for SAM
	mean from the vertices of the label

##### test_accuracy.py
this performs dice loss with a selected model on the dataset


---
## Swarm Learning
Refer to swarm learning github for more information 
	https://github.com/HewlettPackard/swarm-learning/tree/master


in readme > OS, gpu management version, driver version, python version, all other dependencies
docker containers



### Start License Server

get hpe account
run hp install script
https://github.com/HewlettPackard/swarm-learning/blob/master/docs/HPE%20AutoPass%20License%20Server%20User%20Guide.pdf

```
sudo service hpLicenseServer start
```



## Swarm Learning Setup
Detail can be found in this github link
https://github.com/HewlettPackard/swarm-learning/tree/master/examples/mnist
Specific setup found below for SAM



### Initialize
1. _On both host-1 and host-2_:  
    cd to `swarm-learning` folder (i.e. parent to examples directory).
    
2. _On both host-1 and host-2_:  
    Create a temporary `workspace` directory and copy `mnist` example and `gen-cert` utility there.
    
    ```
    mkdir workspace
    cp -r examples/mnist workspace/
    cp -r examples/utils/gen-cert workspace/mnist/
    ```
    
3. _On both host-1 and host-2_:  
    Run the `gen-cert` utility to generate certificates for each Swarm component using the command: `gen-cert -e <EXAMPLE-NAME> -i <HOST-INDEX>`  
    _On host-1_:
    
    ```
    ./workspace/mnist/gen-cert -e mnist -i 1
    ```
    
    _On host-2_:
    
    ```
    ./workspace/mnist/gen-cert -e mnist -i 2
    ```
    
4. _On both host-1 and host-2_:  
    Share the CA certificates between the hosts as follows –  
    _On host-1_:
    
    ```
    scp host-2:<PATH>workspace/mnist/cert/ca/capath/ca-2-cert.pem workspace/mnist/cert/ca/capath
    ```
    
    _On host-2_:
    
    ```
    scp host-1:<PATH>workspace/mnist/cert/ca/capath/ca-1-cert.pem workspace/mnist/cert/ca/capath
    ```
    
```
### Configure environment variables
APLS_IP=10.200.85.134 #ip of server with license
SN_1_IP=10.200.85.134 #ip of server with first swarm node
SN_2_IP=10.200.85.133 #ip of server with second swarm node
HOST_1_IP=10.200.85.134 #same as SN_1_IP in this case 
HOST_2_IP=10.200.85.133 #same as SN_2_IP in this case 

# keep these defaults
SN_API_PORT=30304
SN_P2P_PORT=30303
SL_1_FS_PORT=16000
SL_2_FS_PORT=17000

```


1. _On both host-1 and host-2_:  
    Search and replace all occurrences of placeholders and replace them with appropriate values.
    
    ```
    sed -i "s+<PROJECT-MODEL>+$(pwd)/workspace/mnist/model+g" workspace/mnist/swci/taskdefs/swarm_mnist_task.yaml
    sed -i "s+<SWARM-NETWORK>+host-1-net+g" workspace/mnist/swop/swop1_profile.yaml
    sed -i "s+<SWARM-NETWORK>+host-2-net+g" workspace/mnist/swop/swop2_profile.yaml
    sed -i "s+<HOST_ADDRESS>+${HOST_1_IP}+g" workspace/mnist/swop/swop1_profile.yaml
    sed -i "s+<HOST_ADDRESS>+${HOST_2_IP}+g" workspace/mnist/swop/swop2_profile.yaml
    sed -i "s+<LICENSE-SERVER-ADDRESS>+${APLS_IP}+g" workspace/mnist/swop/swop*_profile.yaml
    sed -i "s+<PROJECT>+$(pwd)/workspace/mnist+g" workspace/mnist/swop/swop*_profile.yaml
    sed -i "s+<PROJECT-CERTS>+$(pwd)/workspace/mnist/cert+g" workspace/mnist/swop/swop*_profile.yaml
    sed -i "s+<PROJECT-CACERTS>+$(pwd)/workspace/mnist/cert/ca/capath+g" workspace/mnist/swop/swop*_profile.yaml
    ```
    
    Note: If the swarm installation directory is different for both the hosts then user need to manually modify the value from the swarm_mnist_task.yaml file to the path that is common to both the hosts.
    
2. _On both host-1 and host-2_:  
    Create a docker volume and copy SwarmLearning wheel file there
    
    ```
    docker volume rm sl-cli-lib
    docker volume create sl-cli-lib
    docker container create --name helper -v sl-cli-lib:/data hello-world
    docker cp -L lib/swarmlearning-client-py3-none-manylinux_2_24_x86_64.whl helper:/data
    docker rm helper
    ```

### Start docker network
```
docker network create host-1-net
docker network create host-2-net
```

### Start Swarm Sentinel Nodes
```
./scripts/bin/run-sn -d --name=sn1 --network=host-1-net --host-ip=${HOST_1_IP} --sentinel --sn-p2p-port=${SN_P2P_PORT} --sn-api-port=${SN_API_PORT} --key=workspace/mnist/cert/sn-1-key.pem --cert=workspace/mnist/cert/sn-1-cert.pem --capath=workspace/mnist/cert/ca/capath --apls-ip=${APLS_IP}
```

```
./scripts/bin/run-sn -d --rm --name=sn2 --network=host-2-net --host-ip=${HOST_2_IP} --sentinel-ip=${SN_1_IP} --sn-p2p-port=${SN_P2P_PORT} --sn-api-port=${SN_API_PORT} --key=workspace/mnist/cert/sn-2-key.pem --cert=workspace/mnist/cert/sn-2-cert.pem --capath=workspace/mnist/cert/ca/capath --apls-ip=${APLS_IP}
```


### Start Swarm Training with image

Host 1
```
./scripts/bin/run-sl -d --name=sl1 --host-ip=${HOST_1_IP} \
--sn-ip=${SN_1_IP} --sn-api-port=${SN_API_PORT} --sl-fs-port=${SL_1_FS_PORT} \
--key=workspace/mnist/cert/sl-1-key.pem \
--cert=workspace/mnist/cert/sl-1-cert.pem \
--capath=workspace/mnist/cert/ca/capath \
--ml-image=sam-swarm:latest --ml-name=ml1 \
--ml-w=/tmp/test --ml-entrypoint=python3 --ml-cmd=/workspace/segment-anything/mnist.py \
--ml-v=workspace/mnist/model:/tmp/test/model \
--ml-e MODEL_DIR=model \
--ml-e SCRATCH_DIR=/tmp/test/model \
--ml-e MAX_EPOCHS=100 \
--ml-e EPOCHS=150 \
--ml-e SYNC=1000 \
--ml-e BATCH_SIZE=50 \
--ml-e PARTITION=0 \
--ml-e MIN_PEERS=2 \
--ml-gpus="all" \
--ml-e https_proxy= \
--ml-e http_proxy= \
--apls-ip=${APLS_IP} \
```

Host 2
```
./scripts/bin/run-sl -d --name=sl2 --host-ip=${HOST_2_IP} \
--sn-ip=${SN_2_IP} --sn-api-port=${SN_API_PORT} --sl-fs-port=${SL_2_FS_PORT} \
--key=workspace/mnist/cert/sl-2-key.pem \
--cert=workspace/mnist/cert/sl-2-cert.pem \
--capath=workspace/mnist/cert/ca/capath \
--ml-image=sam-swarm:latest --ml-name=ml2 \
--ml-w=/tmp/test --ml-entrypoint=python3 --ml-cmd=/workspace/segment-anything/mnist.py \
--ml-v=workspace/mnist/model:/tmp/test/model \
--ml-e MODEL_DIR=model \
--ml-e SCRATCH_DIR=/tmp/test/model \
--ml-e MAX_EPOCHS=100 \
--ml-e EPOCHS=150 \
--ml-e SYNC=1000 \
--ml-e BATCH_SIZE=50 \
--ml-e MIN_PEERS=2 \
--ml-e PARTITION=50 \
--ml-gpus="all" \
--ml-e https_proxy= \
--ml-e http_proxy= \
--apls-ip=${APLS_IP} \
```


### References 
##### SAM
https://github.com/facebookresearch/segment-anything
##### Dataset
https://figshare.com/articles/dataset/brain_tumor_dataset/1512427
##### Swarm Learning 
https://github.com/HewlettPackard/swarm-learning/tree/master/examples/mnist
