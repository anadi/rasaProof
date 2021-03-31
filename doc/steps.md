# Steps in building the model

## 1 Formalize problem 

See First try [Formulation-1.md](Formulation-1.md)

## 2 Initialize the default model

TODO: resolve the initial log statements from model initiation

```
$ docker run -u $(id -u):$(id -g) -v $(pwd):/app rasa/rasa:2.4.3-full init --no-prompt
2021-03-31 02:39:31.132166: W tensorflow/stream_executor/platform/default/dso_loader.cc:59] Could not load dynamic library 'libcudart.so.10.1'; dlerror: libcudart.so.10.1: cannot open shared object file: No such file or directory
2021-03-31 02:39:31.132213: I tensorflow/stream_executor/cuda/cudart_stub.cc:29] Ignore above cudart dlerror if you do not have a GPU set up on your machine.
Matplotlib created a temporary config/cache directory at /tmp/matplotlib-pje7dy0d because the default path (/.config/matplotlib) is not a writable directory; it is highly recommended to set the MPLCONFIGDIR environment variable to a writable directory, in particular to speed up the import of Matplotlib and to better support multiprocessing.
2021-03-31 02:39:35 WARNING  rasa.utils.common  - Failed to write global config. Error: [Errno 13] Permission denied: '/.config'. Skipping.
More log statement
```

## 3 Reduce the models to greet and bot challenge and retrain

### Modify domain.yml

Remove All intents other then greet and bot_challenge.
Remove the related responses.

### Modify nlu.yml

Remove all examples other then greet and bot_challenge

### Modify rules.yml

Remove all rules other then bot_challenge

### Modify stories.yml

Modify to have one happy and one sad path

### Train again

```
$ docker run -u $(id -u):$(id -g) -v $(pwd):/app rasa/rasa:2.4.3-full train
```

### Play with model

Note the -it option, very important:

```
$ docker run -it -u $(id -u):$(id -g) -v $(pwd):/app rasa/rasa:2.4.3-full  shell
```