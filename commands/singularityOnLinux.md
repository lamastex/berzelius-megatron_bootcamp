# Installing Singularity on Linux

From [Quick Start](https://sylabs.io/guides/3.0/user-guide/quick_start.html)...

```
https://golang.org/dl/go1.17.2.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.17.2.linux-amd64.tar.gz 
echo 'export GOPATH=${HOME}/go' >> ~/.bashrc 
echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc
source ~/.bashrc
echo $GOPATH
mkdir -p $GOPATH/src/github.com/sylabs
cd $GOPATH/src/github.com/sylabs
git clone https://github.com/sylabs/singularity.git
cd singularity/
go get -u -v github.com/golang/dep/cmd/dep
cd $GOPATH/src/github.com/sylabs/singularity
./mconfig 
make -C builddir
sudo make -C builddir install
```

Now we have it installed:

```
$ singularity --version
singularity-ce version 3.9.0-rc.1+6-g0bdcf20b6
```

