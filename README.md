# Developing Kubernetes on Mac OS X

Kubernetes comes with a great script that lets you run an entire cluster locally using your current source code tree named `hack/local-up-cluster.sh`. However, it only runs on Linux, which means that if you're developing on your Mac you have to copy your source tree to a Linux machine to use the script (or switch to a Linux machine for development). Go in general, however, runs fine on Mac, so here is the flow that this Vagrant configuration aims to enable:

* Edit source files on your Mac's checkout of Kubernetes and use `go build` (for syntax checking) and `go test` (for unit tests) directly on your Mac.
* Run `vagrant up` (in this directory) to automatically:
 * Launch a Centos 7 VM on your Mac that has Go and Docker installed on the IP 10.1.2.3.
 * Enable the ability to run a Kubernetes cluster using `hack/local-up-cluster.sh`.

# Getting started

You must have the following installed:

* Virtualbox >= 5.1.22
 * Download and install from https://www.virtualbox.org/.
* Vagrant >= 1.9.5
 * Download and install from https://www.vagrantup.com/.
* `vagrant-vbguest` Vagrant plugin.
 * Install by running: `vagrant plugin install vagrant-vbguest`. We use the official CentOS 7 Vagrant box, which does not come with Virtualbox Guest Additions - installing this plugin ensures that the additions are installed on first `vagrant up`.
* Go and a proper GOPATH on your Mac
 * See https://golang.org/doc/code.html for more information.

Next, install Kubernetes to your GOPATH by running `go get k8s.io/kubernetes/...`. If you want to write and contribute code, fork Kubernetes with your user on GitHub, and add your repo as a remote to your local checkout by running:

Once you have a Kubernetes checkout in your GOPATH:

1. `git clone` this repo (it does not need to be in your GOPATH) and `cd` into it.
1. Run `vagrant up`. That will start up the VM and bootstrap it with docker, go, and your $GOPATH to /go (amongst other things; see [setup.sh](setup.sh) for the complete bootstrapping process).
1. Use `vagrant ssh` to SSH into the VM.
1. Enter `sudo -i /go/src/k8s.io/kubernetes/hack/local-up-cluster.sh` to start up a cluster using the code in your checkout.

The kubernetes apiserver is run on 10.1.2.3, not on 127.0.0.1, in order to enable access from your OS X host machine. If you want to use kubectl from your Mac, run `export KUBERNETES_MASTER=10.1.2.3:8080` (the VM's environment is already preconfigured as such).
