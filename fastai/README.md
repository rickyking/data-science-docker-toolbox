# Fast.ai Docker files

This docker file is based on [https://github.com/Paperspace/fastai-docker]().

## Run the docker

Put the following in `~/.bashrc` or `~/.zshrc`: `alias fastai="nvidia-docker run --init --rm -d -p 8889:8888 -v ~/dl/code:/code -v ~/dl/data:/data -w="/code/fastai" rickyking/fastai"`

Or simply run following command in `bash`: `nvidia-docker run --init --rm -d -p 8889:8888 -v ~/dl/code:/code -v ~/dl/data:/data -w="/code/fastai" rickyking/fastai`

Explanation:

1. `nvidia-docker` is running nvidia docker runtime
2. `--init`is running an init inside the container that forwards signals and reaps processes
3. `--rm` is removing the stopped container
4. `-d` is running the container in background
5. `-p` is port forwarding `port_of_host:port_of_container`
6. `-v` is binding volume, which helps persit the code and data
7. `-w` is telling the working directory

The container will launch the jupyter lab and notebook automatically.