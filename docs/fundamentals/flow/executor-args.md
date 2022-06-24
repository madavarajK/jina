| Name | Description | Type | Default |
|----|----|----|----|
| `name` | The name of this object.<br><br>    This will be used in the following places:<br>    - how you refer to this object in Python/YAML/CLI<br>    - visualization<br>    - log message header<br>    - ...<br><br>    When not given, then the default naming strategy will apply. | `string` | `None` |
| `workspace` | The working directory for any IO operations in this object. If not set, then derive from its parent `workspace`. | `string` | `None` |
| `log_config` | The YAML config of the logger used in this object. | `string` | `default` |
| `quiet` | If set, then no log will be emitted from this object. | `boolean` | `False` |
| `quiet_error` | If set, then exception stack information will not be added to the log | `boolean` | `False` |
| `timeout_ctrl` | The timeout in milliseconds of the control request, -1 for waiting forever | `number` | `60` |
| `polling` | The polling strategy of the Deployment and its endpoints (when `shards>1`).<br>    Can be defined for all endpoints of a Deployment or by endpoint.<br>    Define per Deployment:<br>    - ANY: only one (whoever is idle) Pod polls the message<br>    - ALL: all Pods poll the message (like a broadcast)<br>    Define per Endpoint:<br>    JSON dict, {endpoint: PollingType}<br>    {'/custom': 'ALL', '/search': 'ANY', '*': 'ANY'} | `string` | `ANY` |
| `uses` | The config of the executor, it could be one of the followings:<br>        * an Executor YAML file (.yml, .yaml, .jaml)<br>        * a Jina Hub Executor (must start with `jinahub://` or `jinahub+docker://`)<br>        * a docker image (must start with `docker://`)<br>        * the string literal of a YAML config (must start with `!` or `jtype: `)<br>        * the string literal of a JSON config<br><br>        When use it under Python, one can use the following values additionally:<br>        - a Python dict that represents the config<br>        - a text file stream has `.read()` interface | `string` | `BaseExecutor` |
| `uses_with` | Dictionary of keyword arguments that will override the `with` configuration in `uses` | `object` | `None` |
| `uses_metas` | Dictionary of keyword arguments that will override the `metas` configuration in `uses` | `object` | `None` |
| `uses_requests` | Dictionary of keyword arguments that will override the `requests` configuration in `uses` | `object` | `None` |
| `py_modules` | The customized python modules need to be imported before loading the executor<br><br>Note that the recommended way is to only import a single module - a simple python file, if your<br>executor can be defined in a single file, or an ``__init__.py`` file if you have multiple files,<br>which should be structured as a python package. For more details, please see the<br>`Executor cookbook <https://docs.jina.ai/fundamentals/executor/executor-files/>`__ | `array` | `None` |
| `port` | The port for input data to bind to, default is a random port between [49152, 65535] | `number` | `random in [49152, 65535]` |
| `host_in` | The host address for binding to, by default it is 0.0.0.0 | `string` | `0.0.0.0` |
| `native` | If set, only native Executors is allowed, and the Executor is always run inside WorkerRuntime. | `boolean` | `False` |
| `output_array_type` | The type of array `tensor` and `embedding` will be serialized to.<br><br>Supports the same types as `docarray.to_protobuf(.., ndarray_type=...)`, which can be found <br>`here <https://docarray.jina.ai/fundamentals/document/serialization/#from-to-protobuf>`.<br>Defaults to retaining whatever type is returned by the Executor. | `string` | `None` |
| `entrypoint` | The entrypoint command overrides the ENTRYPOINT in Docker image. when not set then the Docker image ENTRYPOINT takes effective. | `string` | `None` |
| `docker_kwargs` | Dictionary of kwargs arguments that will be passed to Docker SDK when starting the docker '<br>container. <br><br>More details can be found in the Docker SDK docs:  https://docker-py.readthedocs.io/en/stable/ | `object` | `None` |
| `volumes` | The path on the host to be mounted inside the container. <br><br>Note, <br>- If separated by `:`, then the first part will be considered as the local host path and the second part is the path in the container system. <br>- If no split provided, then the basename of that directory will be mounted into container's root path, e.g. `--volumes="/user/test/my-workspace"` will be mounted into `/my-workspace` inside the container. <br>- All volumes are mounted with read-write mode. | `array` | `None` |
| `gpus` | This argument allows dockerized Jina executor discover local gpu devices.<br><br>    Note, <br>    - To access all gpus, use `--gpus all`.<br>    - To access multiple gpus, e.g. make use of 2 gpus, use `--gpus 2`.<br>    - To access specified gpus based on device id, use `--gpus device=[YOUR-GPU-DEVICE-ID]`<br>    - To access specified gpus based on multiple device id, use `--gpus device=[YOUR-GPU-DEVICE-ID1],device=[YOUR-GPU-DEVICE-ID2]`<br>    - To specify more parameters, use `--gpus device=[YOUR-GPU-DEVICE-ID],runtime=nvidia,capabilities=display | `string` | `None` |
| `disable_auto_volume` | Do not automatically mount a volume for dockerized Executors. | `boolean` | `False` |
| `host` | The host address of the runtime, by default it is 0.0.0.0. | `string` | `0.0.0.0` |
| `quiet_remote_logs` | Do not display the streaming of remote logs on local console | `boolean` | `False` |
| `upload_files` | The files on the host to be uploaded to the remote<br>workspace. This can be useful when your Deployment has more<br>file dependencies beyond a single YAML file, e.g.<br>Python files, data files.<br><br>Note,<br>- currently only flatten structure is supported, which means if you upload `[./foo/a.py, ./foo/b.pp, ./bar/c.yml]`, then they will be put under the _same_ workspace on the remote, losing all hierarchies.<br>- by default, `--uses` YAML file is always uploaded.<br>- uploaded files are by default isolated across the runs. To ensure files are submitted to the same workspace across different runs, use `--workspace-id` to specify the workspace. | `array` | `None` |
| `runtime_cls` | The runtime class to run inside the Pod | `string` | `WorkerRuntime` |
| `timeout_ready` | The timeout in milliseconds of a Pod waits for the runtime to be ready, -1 for waiting forever | `number` | `600000` |
| `env` | The map of environment variables that are available inside runtime | `object` | `None` |
| `shards` | The number of shards in the deployment running at the same time. For more details check https://docs.jina.ai/fundamentals/flow/create-flow/#complex-flow-topologies | `number` | `1` |
| `replicas` | The number of replicas in the deployment | `number` | `1` |
| `monitoring` | If set, spawn an http server with a prometheus endpoint to expose metrics | `boolean` | `False` |
| `port_monitoring` | The port on which the prometheus server is exposed, default is a random port between [49152, 65535] | `number` | `random in [49152, 65535]` |
| `retries` | Number of retries per gRPC call. If <0 it defaults to max(3, num_replicas) | `number` | `-1` |
| `install_requirements` | If set, install `requirements.txt` in the Hub Executor bundle to local | `boolean` | `False` |
| `force_update` | If set, always pull the latest Hub Executor bundle even it exists on local | `boolean` | `False` |
| `compression` | The compression mechanism used when sending requests from the Head to the WorkerRuntimes. For more details, check https://grpc.github.io/grpc/python/grpc.html#compression. | `string` | `None` |
| `uses_before_address` | The address of the uses-before runtime | `string` | `None` |
| `uses_after_address` | The address of the uses-before runtime | `string` | `None` |
| `connection_list` | dictionary JSON with a list of connections to configure | `string` | `None` |
| `disable_reduce` | Disable the built-in reduce mechanism, set this if the reduction is to be handled by the Executor connected to this Head | `boolean` | `False` |
| `timeout_send` | The timeout in milliseconds used when sending data requests to Executors, -1 means no timeout, disabled by default | `number` | `None` |