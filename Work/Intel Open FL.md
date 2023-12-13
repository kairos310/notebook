# Docker Setup

Find open ports
``` bash
DIRECTOR=$(ss -tulpn | grep LISTEN | grep -Po -m 1 '(?<=127.0.0.1:)\d{5}')
```
pull image if does not exist
```bash
docker pull intel/openfl  
#docker run -it --network host intel/openfl bash
```
Run with open port
```
docker run -it --network host -e DIRECTOR=$DIRECTOR intel/openfl bash
  ```

```
cd /openfl/openfl-tutorials/interactive_api/PyTorch_MedMNIST_2D/director  
pip install -r requirements.txt  
./start_director.sh
```

```
cd /openfl/openfl-tutorials/interactive_api/PyTorch_MedMNIST_2D/envoy  
pip install -r requirements.txt  
./start_envoy.sh envoy_name envoy_config.yaml  
```

```bash
sed -i 's/\(listen_port: \) [0-9]*/\1 '"$DIRECTOR"'/' director_config.yaml
DIRECTOR=12001
sed -i 's/\(-dp\) [0-9]*/\1 '"$DIRECTOR"'/' start_envoy.sh
```

```
pip install notebook
python -m notebook Pytorch_MedMNIST_2D.ipynb --port=8888 --allow-root --no-browser
```


This is what should print out, copy one of these links
```
To access the server, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/jpserver-32-open.html
Or copy and paste one of these URLs:
        http://localhost:8888/tree?token=6b8b84a53e96ba3ae2ad93226e5af7f508f6b09af18292f7
        http://127.0.0.1:8888/tree?token=6b8b84a53e96ba3ae2ad93226e5af7f508f6b09af18292f7

```

On a separate terminal
```
ssh -L 8888:localhost:8888 zwong@splprisn02.stjude.org
```

```
ssh -L 8888:localhost:8888 zwong@10.42.24.55
```
Then open up your browser of choice and 

```
docker run -it nvflare/nvflare-dev bash
```
