# Kubernetes Sandpit
Sample project to install `minikube` and deploy a number of kubernetes artifacts to demonstrate functionality.

*Note: to initialise the repository correctly, run the following to correctly fetch the submodules required:*
```bash
git submodule update --init --recursive
```

Refer to:
* [installing and configuring `minikube`](_doc/01-install.md)
* [building `golang` and `Java` APIs and packaging as containers](_doc/02-build.md)
* [deploying the previously built APIs in `minikube`](_doc/03-deploy.md)