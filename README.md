# Hydra in Docker
This repo contains a Dockerfile that can be used to build an image containing the compiled Hydra Software with all dependencies.

## Basic Usage
> **_NOTE:_** If you are on Linux and have trouble running docker commands without `sudo`, check [this](https://docs.docker.com/engine/install/linux-postinstall/).

Build:
```bash
docker build -t hydrabase -f DockerfileHydraBase .
```

Run interactively:
```bash
docker run -it --rm --name=hydrabase hydrabase bash
```

Save to .tar:
```bash
docker save hydrabase > hydrabase.tar
```

Load from .tar:
```bash
docker load < hydrabase.tar
```

Run preprocessing example:
```bash
docker run --name=hydrarun hydrabase /hydra/src/hydra_G \
  --bed-to-sparse \
  --bfile /hydra/example/t_M10K_N_5K \
  --pheno /hydra/example/normal.phen \
  --blocks-per-rank 4 \
  --sparse-dir /hydra/example \
  --sparse-basename t_M10K_N_5K
```

Copy result file from container to host after the run:
```bash
docker cp hydrarun:/hydra/example/t_M10K_N_5K.log ./
```


## Usage in UKB DNA-Nexus
There seem to be multiple ways a docker container can be used on UKB DNA-Nexus. One way is to use a Cloud Workstation in Container [(CWIC)](https://documentation.dnanexus.com/developer/cloud-workstations/cwic), from where one can run interactive shells and/or submit batch jobs. Another way is to create an Applet that loads the container and performs some computation. I will be focussing on that option in the latter.

In order to use this image with an applet on the DNA-Nexus platform, we need to build it, save it to `.tar`.
We can then build an Applet that uses this image to compute something. This can be done for example following the [tutorial](https://documentation.dnanexus.com/developer/apps/intro-to-building-apps) in the DNA-Nexus docs.
For example, one could run the preprocessing example above on the DNA-Nexus platform with a `dxapp.json` and a bash executable like `hydra-preprocess-test.sh`. It uses the `.tar` of the image produced with the commands above. For it to be available in the Applet, one needs to move it into the `applet_name/resources` directory that is created by the `dx-build-wizard`. The contents of this directory are moved into and made available in the root directory of the fresh Ubuntu instance that the Applet runs on (see the docs [here](https://documentation.dnanexus.com/developer/apps/app-build-process#the-resources-directory-and-its-use)).