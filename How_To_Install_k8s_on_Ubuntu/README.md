# How To Install Kubernetes on Ubuntu 18.04 :pencil2:

* [Youtube Tutorial - How To Install Kubernetes on Ubuntu 18.04](https://youtu.be/Uz4dSmJbGEE)

I wrote this article [How to install Kubernetes on windows 10](https://github.com/twtrubiks/k8s-tutorial/tree/master/How_to_install_k8s_on_win10) a year ago, but I later found that many things are very troublesome to install on Windows, so today I write an article on how to install k8s on ubuntu:smile:

## First step - Install kubectl

[Install kubectl on Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

Download the latest release

```cmd
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```

Make the kubectl binary executable

```cmd
chmod +x ./kubectl
```

Move the binary in to your PATH.

```cmd
sudo mv ./kubectl /usr/local/bin/kubectl
```

Test if the installation was successful

```cmd
kubectl version
```

The following message appears

![alt tag](https://i.imgur.com/K4yvehI.png)

or the following message appears

```cmd
Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.0", GitCommit:"70132b0f130acc0bed193d9ba59dd186f0e634cf", GitTreeState:"clean", BuildDate:"2019-12-07T21:20:10Z", GoVersion:"go1.13.4", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

[Verifying kubectl configuration](https://kubernetes.io/docs/tasks/tools/install-kubectl/#verifying-kubectl-configuration)

```cmd
kubectl cluster-info
```

This is normal because Minikube has not been installed

[Optional kubectl configurations](https://kubernetes.io/docs/tasks/tools/install-kubectl/#optional-kubectl-configurations)

Integration zsh

```cmd
sudo vim ~/.zshrc
```

add `kubectl` to your `~/.zshrc` file

![alt tag](https://i.imgur.com/4sUtAO3.png)

Restart Terminal, then Autocomplete using the Tab key

![alt tag](https://i.imgur.com/8rTR6zX.png)

## The second step - Prepare Install Minikube

[Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)

To check if virtualization is supported on Linux, run the following command and verify that the output is non-empty:

```cmd
grep -E --color 'vmx|svm' /proc/cpuinfo
```

If there is a red text, it means virtualization is supported on your Linux.

![alt tag](https://i.imgur.com/HpLnZaj.png)

Before installing Minikube, we first install VirtualBox

[Install VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)

Enter virtualbox to open automatically

```cmd
virtualbox
```

Reminder:exclamation::exclamation:

If your operating system is Windows, then you install VirtualBox, and
install Ubuntu in VirtualBox, and then if you install VirtualBox in Ubuntu, you will find that you cannot use the following command

```cmd
minikube start --vm-driver=virtualbox
```

Because it's no longer possible virtualization in virtualization,


You can only use the following command

```cmd
minikube start --vm-driver=none
```

Reference [specifying-the-vm-drive](https://kubernetes.io/docs/setup/learning-environment/minikube/#specifying-the-vm-driver)

## The third step - Install Minikube

Continue to install Minikube

[Install Minikube via direct download](https://kubernetes.io/docs/tasks/tools/install-minikube/#install-minikube-via-direct-download)


download a stand-alone binary and use that

```cmd
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
```

Here’s an easy way to add the Minikube executable to your path:

```cmd
sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/
```

Start with virtualbox by default

```cmd
minikube start --vm-driver=virtualbox
```

This step will take a long time, please be patient

![alt tag](https://i.imgur.com/6D2Fx70.png)

Enter the following command again,

```cmd
kubectl cluster-info
```

This time the message will appear

![alt tag](https://i.imgur.com/umFBo6p.png)

## Environment

* Ubuntu 18.04.3

## Reference

* [kubernetes-docs](https://kubernetes.io/docs/home/)

## Donation

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[Sponsor payments](http://bit.ly/2F7Jrha)

O'Pay

![alt tag](https://i.imgur.com/LRct9xa.png)

[Sponsor payments](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## Sponsored List

[Sponsored List](https://github.com/twtrubiks/Thank-you-for-donate)
