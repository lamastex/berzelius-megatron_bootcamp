# berzelius-megatron_bootcamp
This is a public repository of the megatron bootcamp for GPUs on berzelius supercomputer for AI in Sweden

# Setup

## Access to Berzelius

- get access to berzelius by following their instructions in the invitation
  - assuming you have access via `$ ssh -X x_raasa@berzelius1.nsc.liu.se`

## thinlinc

- get thinlinc client for accessing the GUIs for jupyter via firefox, profiler, etc.
    - See [Chapter 8, Client Platforms](https://www.cendio.com/resources/docs/tag/clientplatforms.html) for instructions on how to start the client on different platforms. 
    - download from: [https://www.cendio.com/thinlinc/download](https://www.cendio.com/thinlinc/download) and run thinlinc client
      - for Linux see [https://www.cendio.com/resources/docs/tag/linuxclients.html](https://www.cendio.com/resources/docs/tag/linuxclients.html)  
      - `wget https://www.cendio.com/downloads/clients/thinlinc-client_4.13.0-2172_amd64.deb`
      - install: `$ sudo dpkg -i thinlinc-client_4.13.0-2172_amd64.deb`  
      - run: `$ /opt/thinlinc/bin/tlclient`, fill in username: `x_raasa` and server: `berzelius1.nsc.liu.se` and do <F-8> to exit full-screen. 
      - If all goes well, you should see the thnilinc client with shell, browser and file manager as in [this image](images/ThinLincClientToberzelius.png)
    - for details read [https://www.cendio.com/resources/docs/tag/client.html](https://www.cendio.com/resources/docs/tag/client.html)
