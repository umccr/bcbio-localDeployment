# bcbio-localDeployment

The bcbio documentation mentions "bcbio should install cleanly on Linux systems. For Mac OSX, we suggest trying bcbio-vm which runs bcbio on Amazon Web Services or isolates all the third party tools inside a Docker container. bcbio-vm is still a work in progress but not all of the dependencies bcbio uses install cleanly on OSX."

I ran into issues while trying to use bcbio-vm. The following command ```~/install/bcbio-vm/anaconda/bin/conda install --yes -c conda-forge -c bioconda bcbio-nextgen-vm``` threw TypeError ```CondaMemoryError does not take keyword arguments``` inside docker. 

Vlad suggested to try a normal install inside a virtual machine.

## Docker

Initially started working with VirtualBox and Docker - downloaded both applications for Mac.
But later on, ran into storage issues. Roman suggested that VirtualBox is now deprecated and suggested using *just* docker.

Following are the final steps that are **working** now.

1. Initialize docker.  
  - **IMP** The test data is huge - 100G. So alter docker setting to increase storage/disk space from ~65G to ~250G.

Swith to commandline

2. docker pull bcbio/bcbio

3. docker images (to check if bcbio image is available)

4. docker run -td image_name 

5. docer ps (list running containers)

6. docker exec -it <container_id> /bin/bash (to open a bash shell in the container)

7. Hopefully bcbio will be in the path now

8. Follow the instructions from documentation to get the test data and test the pipeline (https://bcbio-nextgen.readthedocs.io/en/latest/contents/testing.html#rnaseq-example) 
  . Test data for RNA seq takes quite a while to download (few hours, so be prepared).  
