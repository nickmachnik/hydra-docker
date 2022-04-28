# Hydra in Docker

This repo contains a Dockerfile that can be used to build an image containing the compiled Hydra Software with all dependencies.

Build:

```bash
docker build -t hydra_g -f DockerfileHydraBase .
```

Run interactively:
```bash
docker run -it --rm --name=hydra_g hydra_g bash
```

Save to .tar:

```bash
docker save hydra_g > hydrabase.tar
```
