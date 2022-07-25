# Fairseq on Amazon SageMaker

[Fairseq](https://github.com/pytorch/fairseq) Fairseq(-py) is a sequence modeling toolkit that allows researchers and developers to train custom models for translation, summarization, language modeling and other text generation tasks.

In this repository, we will show how to integrate Fairseq into Amazon SageMaker Training Job using Pytorch Estimator. Instead of using Custom Docker container, this example uses shell script as an entry point of Pytorch estimator. Which contains dependancy installation commands and data preprocessing commands.


### Example notebooks
* `w2v_finetuning.ipynb`: [Fine-tune a pre-trained wav2vec 2.0 model example](https://github.com/facebookresearch/fairseq/tree/main/examples/wav2vec) of wav2vec using Librispeech dataset


### Local Mode
In case of using local mode, we recommend using the following command as a startup script of SageMaker Notebook to change the docker repository path.

```console

#!/bin/bash

set -ex

DAEMON_PATH="/etc/docker"
MEMORY_SIZE=10G

FLAG=$(cat $DAEMON_PATH/daemon.json | jq 'has("data-root")')
# echo $FLAG

if [ "$FLAG" == true ]; then
    echo "Already revised"
else
    echo "Add data-root and default-shm-size=$MEMORY_SIZE"
    sudo cp $DAEMON_PATH/daemon.json $DAEMON_PATH/daemon.json.bak
    sudo cat $DAEMON_PATH/daemon.json.bak | jq '. += {"data-root":"/home/ec2-user/SageMaker/.container/docker","default-shm-size":"'$MEMORY_SIZE'"}' | sudo tee $DAEMON_PATH/daemon.json > /dev/null
    sudo service docker restart
    echo "Docker Restart"
fi

```
