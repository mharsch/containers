# fah-amd-pro - Folding@home GPU Container for AMDGPU-PRO

This file covers only information specific to the `fah-amd-pro` container.
Read the README and CONTRIBUTING at
<https://github.com/foldingathome/containers/> for design goals,
architecture, guidelines for contributing, and other information.

### Technical Requirements

* Docker 19.03 or later (for single node).
* Persistent storage for each running container.

#### For AMD GPUs
* Install AMDGPU-PRO drivers on the host per the instructions
  [here](http://amdgpu-install.readthedocs.io)

## Running on a Single machine

### Start Folding on Single Machine

```bash
# Run container with GPUs, name it "fah0", map user and /fah volume
docker run --device=/dev/kfd --device=/dev/dri \
  --security-opt seccomp=unconfined --group-add video --name fah2 \
  -d --user "$(id -u):$(id -g)" --volume $HOME/fah:/fah fah-amd-pro
```

#### 1-GPU, 1-CPU, 16 thread Example Config

```xml
<config>
  <!-- Set with your user, passkey, team-->
  <user value="Anonymous"/>
  <passkey value=""/>
  <team value="0"/>

  <power value="full"/>
  <exit-when-done v='true'/>

  <web-enable v='false'/>
  <disable-viz v='true'/>
  <gui-enabled v='false'/>

  <!-- 1 slots for GPUs -->
  <slot id='0' type='GPU'> </slot>

  <!-- 16-1 = 15 = 3*5 for decomposition -->
  <slot id='1' type='SMP'> <cpus v='15'/> </slot>

</config>
```
