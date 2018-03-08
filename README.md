# bcbio-localDeployment

The bcbio documentation mentions "bcbio should install cleanly on Linux systems. For Mac OSX, we suggest trying bcbio-vm which runs bcbio on Amazon Web Services or isolates all the third party tools inside a Docker container. bcbio-vm is still a work in progress but not all of the dependencies bcbio uses install cleanly on OSX."

I ran into issues while trying to use bcbio-vm. The following command ```~/install/bcbio-vm/anaconda/bin/conda install --yes -c conda-forge -c bioconda bcbio-nextgen-vm``` threw TypeError ```CondaMemoryError does not take keyword arguments``` inside docker. 

Vlad suggested to try a normal install inside a virtual machine.

## Docker

Initially started working with VirtualBox and Docker - downloaded both applications for Mac.
But later on, ran into storage issues. Roman suggested that VirtualBox is now deprecated and suggested using *just* docker.

Following are the final steps that are **working** now.

1. Initialize docker.  
  - **IMP:** The test data is huge - 100G. So alter docker setting to increase storage/disk space from ~65G to ~250G.

Swith to commandline

2. docker pull bcbio/bcbio

3. docker images (to check if bcbio image is available)

4. docker run -td image_name 

5. docer ps (list running containers)

6. docker exec -it <container_id> /bin/bash (to open a bash shell in the container)

7. Hopefully bcbio will be in the path now

8. Follow the instructions from documentation to get the test data and test the pipeline (https://bcbio-nextgen.readthedocs.io/en/latest/contents/testing.html#rnaseq-example) 
  . Test data for RNA seq takes quite a while to download (few hours, so be prepared).  


## Issues

Encountered many issues while trying to run a test workflow

* Could not find input system configuration file bcbio_system.yaml

  * https://github.com/chapmanb/bcbio-nextgen/issues/1398 and https://github.com/chapmanb/bcbio-nextgen/issues/2091 suggested to use bcbio-vm on mac.
  * https://github.com/chapmanb/bcbio-nextgen-vm suggests this is still a work in progress
  
* bcbio-nextgen-vm also takes care of reference data used in the pipeline

* Used conda to install bcbio-nextgen-vm
  `conda create -n py2 python=2`
  
  `conda activate py2`
  
  `~/install/bcbio-vm/anaconda/bin/conda install --yes -c conda-forge -c bioconda bcbio-nextgen-vm` (command from documenation https://github.com/chapmanb/bcbio-nextgen-vm) 
  
  `ln -sf /usr/local/share/bcbio-nextgen/config /root/miniconda3/envs/galaxy` ---> Solved 
Could not find input system configuration file bcbio_system.yaml
  
  `bcbio_vm.py --datadir=~/install/bcbio-vm/data install --data --tools --genomes GRCh37 --aligners bwa (to get reference data)`
  
  ```
  Retrieving bcbio-nextgen docker image with code and tools 
  Traceback (most recent call last):
  File "/root/miniconda3/envs/py2/bin/bcbio_vm.py", line 334, in <module>
    args.func(args)
  File "/root/miniconda3/envs/py2/bin/bcbio_vm.py", line 41, in cmd_install
    install.full(args, devel.DOCKER)
  File "/root/miniconda3/envs/py2/lib/python2.7/site-packages/bcbiovm/docker/install.py", line 26, in full
    pull(args, dockerconf)
  File "/root/miniconda3/envs/py2/lib/python2.7/site-packages/bcbiovm/docker/install.py", line 77, in pull
    subprocess.check_call(["docker", "pull", args.image])
  File "/root/miniconda3/envs/py2/lib/python2.7/subprocess.py", line 181, in check_call
    retcode = call(*popenargs, **kwargs)
  File "/root/miniconda3/envs/py2/lib/python2.7/subprocess.py", line 168, in call
    return Popen(*popenargs, **kwargs).wait()
  File "/root/miniconda3/envs/py2/lib/python2.7/subprocess.py", line 390, in __init__
    errread, errwrite)
  File "/root/miniconda3/envs/py2/lib/python2.7/subprocess.py", line 1025, in _execute_child
    raise child_exception
    OSError: [Errno 2] No such file or directory
```




