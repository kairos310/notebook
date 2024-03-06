```
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
2023-12-15 21:11:19,209 - IntimeModelSelector - INFO - model selection weights control: None
2023-12-15 21:11:19,212 - JsonScanner - ERROR - Traceback (most recent call last):
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 98, in _do_scan
    node.processor.process_element(node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 135, in process_element
    self.process_config_element(self.config_ctx, node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_json_config.py", line 110, in process_config_element
    workflow = self.authorize_and_build_component(element, config_ctx, node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 104, in authorize_and_build_component
    return self.build_component(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_json_config.py", line 140, in build_component
    t = super().build_component(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/component_builder.py", line 79, in build_component
    class_path = self.get_class_path(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/component_builder.py", line 114, in get_class_path
    raise ConfigError('Cannot find component class "{}"'.format(class_name))
nvflare.fuel.common.excepts.ConfigError: Cannot find component class "FedAvg"

Error processing config ['/tmp/nvflare/sim_cifar10/cifar10_central/simulate_job/app_server/config/config_fed_server.json']: ConfigError: Error processing ['/tmp/nvflare/sim_cifar10/cifar10_central/simulate_job/app_server/config/config_fed_server.json'] in JSON element {'id': 'fedavg_ctl', 'name': 'FedAvg', 'args': {'min_clients': 1, 'num_rounds': 1, 'persistor_id': 'persistor'}}: path: workflows.#1, exception: ConfigError: Cannot find component class "FedAvg"
2023-12-15 21:11:19,212 - SimulatorRunner - ERROR - FL server execution exception: ConfigError: Error processing ['/tmp/nvflare/sim_cifar10/cifar10_central/simulate_job/app_server/config/config_fed_server.json'] in JSON element {'id': 'fedavg_ctl', 'name': 'FedAvg', 'args': {'min_clients': 1, 'num_rounds': 1, 'persistor_id': 'persistor'}}: path: workflows.#1, exception: ConfigError: Cannot find component class "FedAvg"
Traceback (most recent call last):
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 98, in _do_scan
    node.processor.process_element(node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 135, in process_element
    self.process_config_element(self.config_ctx, node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_json_config.py", line 110, in process_config_element
    workflow = self.authorize_and_build_component(element, config_ctx, node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 104, in authorize_and_build_component
    return self.build_component(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_json_config.py", line 140, in build_component
    t = super().build_component(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/component_builder.py", line 79, in build_component
    class_path = self.get_class_path(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/component_builder.py", line 114, in get_class_path
    raise ConfigError('Cannot find component class "{}"'.format(class_name))
nvflare.fuel.common.excepts.ConfigError: Cannot find component class "FedAvg"

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_app_runner.py", line 70, in start_server_app
    conf.configure()
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 132, in configure
    raise e
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 129, in configure
    self._do_configure()
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 122, in _do_configure
    self.json_scanner.scan(self)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 164, in scan
    self._do_scan(node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 137, in _do_scan
    self._do_scan(_child_node(node, k, 0, v))
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 140, in _do_scan
    self._do_scan(_child_node(node, node.key, i + 1, element[i]))
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 119, in _do_scan
    raise ConfigError(
nvflare.fuel.common.excepts.ConfigError: Error processing ['/tmp/nvflare/sim_cifar10/cifar10_central/simulate_job/app_server/config/config_fed_server.json'] in JSON element {'id': 'fedavg_ctl', 'name': 'FedAvg', 'args': {'min_clients': 1, 'num_rounds': 1, 'persistor_id': 'persistor'}}: path: workflows.#1, exception: ConfigError: Cannot find component class "FedAvg"
2023-12-15 21:11:19,212 - SimulatorServer - INFO - Server app stopped.


Exception in thread Thread-8:
Traceback (most recent call last):
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 98, in _do_scan
    node.processor.process_element(node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 135, in process_element
    self.process_config_element(self.config_ctx, node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_json_config.py", line 110, in process_config_element
    workflow = self.authorize_and_build_component(element, config_ctx, node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 104, in authorize_and_build_component
    return self.build_component(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_json_config.py", line 140, in build_component
    t = super().build_component(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/component_builder.py", line 79, in build_component
    class_path = self.get_class_path(config_dict)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/component_builder.py", line 114, in get_class_path
    raise ConfigError('Cannot find component class "{}"'.format(class_name))
nvflare.fuel.common.excepts.ConfigError: Cannot find component class "FedAvg"

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/lib/python3.8/threading.py", line 932, in _bootstrap_inner
    self.run()
  File "/usr/lib/python3.8/threading.py", line 870, in run
    self._target(*self._args, **self._kwargs)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/app/simulator/simulator_runner.py", line 423, in start_server_app
    server_app_runner.start_server_app(
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_app_runner.py", line 83, in start_server_app
    raise e
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/fed/server/server_app_runner.py", line 70, in start_server_app
    conf.configure()
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 132, in configure
    raise e
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 129, in configure
    self._do_configure()
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/private/json_configer.py", line 122, in _do_configure
    self.json_scanner.scan(self)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 164, in scan
    self._do_scan(node)
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 137, in _do_scan
    self._do_scan(_child_node(node, k, 0, v))
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 140, in _do_scan
    self._do_scan(_child_node(node, node.key, i + 1, element[i]))
  File "/cnvrg/nvflare-env/lib/python3.8/site-packages/nvflare/fuel/utils/json_scanner.py", line 119, in _do_scan
    raise ConfigError(
nvflare.fuel.common.excepts.ConfigError: Error processing ['/tmp/nvflare/sim_cifar10/cifar10_central/simulate_job/app_server/config/config_fed_server.json'] in JSON element {'id': 'fedavg_ctl', 'name': 'FedAvg', 'args': {'min_clients': 1, 'num_rounds': 1, 'persistor_id': 'persistor'}}: path: workflows.#1, exception: ConfigError: Cannot find component class "FedAvg"```

```


## Guideline for joining the federation

install docker 
podman
minikube 
kubectl 
setup kubeconfig 
ssh into cnvrg


access to the kubernetes cluster? 
https://app.cnvrg.io/docs/guides/ssh.html
