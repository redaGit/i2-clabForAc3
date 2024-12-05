# VM-based nodes in containerlab

VM nodes integration in containerlab is based on the [hellt/vrnetlab](https://github.com/hellt/vrnetlab) project which is a fork of `vrnetlab/vrnetlab` where things were added to make it work with the container networking.

Start with cloning the project:

```bash
cd ~ && git clone https://github.com/hellt/vrnetlab.git && \
cd ~/vrnetlab
```

## Building SR OS container image

SR OS VM image is located at `~/images/sros-vm-24.7.R1.qcow2` on your VM and should be copied to the `~/vrnetlab/sros/` directory before building the container image.

```bash
cp ~/images/sros-vm-24.7.R1.qcow2 ~/vrnetlab/sros/
```

Once copied, we can enter in the `~/vrnetlab/sros` image and build the container image:

```bash
cd ~/vrnetlab/sros && make
```

The resulting image will be tagged as `vrnetlab/nokia_sros:24.7.R1`. This can be verified using `docker images` command.

```bash
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
vrnetlab/nokia_sros           24.7.R1   553e94475c12   7 seconds ago   889MB
```

## Deploying the VM-based nodes lab

With the sros image built, we can proceed with the lab deployment. We will deploy a multi-node lab with SR OS, SR Linux and Cisco XRd images. Containerlab makes it possible to have a VM based docker node and a native docker node in the same lab.

This is how the topology looks like:



First, let's switch back to the lab directory:

```bash
cd ~/i2-clab/20-vm
```

Now lets deploy the lab:

```bash
sudo clab dep -c
```

At the end of the deployment, the following table will be displayed. Wait for the sros boot to be completed (see next section), before trying to login to sros.

```bash

```

### Monitoring the boot process

To monitor the boot process of SR OS nodes, you can open a new terminal and run the following command:

```bash
sudo docker logs -f pe1
```

## Connecting to the nodes

To connect to SR OS node:

```bash
ssh admin@pe1-sr1
```

To connect to SR Linux node:

```bash
ssh pe2-srL
```

To connect to Cisco XRd node:

```bash
ssh p4-xrd
```

Refer to the passwords in your card.

## Configuring the nodes

In the Containerlab topology file, we also specified a `startup-config` for each node.

You may refer to the startup configs here.

Each startup config is performing the following configs:

- Configuring interfaces (to other routers and to the client)
- Configuring ISIS
- Configuring Segment Routing over ISIS (SR-ISIS)
- Configuring BGP between PE1 and PE2 system loopback IPs
- Configuring an EVPN service between PE1 and PE2 with SR-ISIS as the tunnel




--- 10.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 1.965/2.378/3.168/0.558 ms
```

We have now completed the section on bring VM based nodes into Containerlab.
