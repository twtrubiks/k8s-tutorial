# How to install Kubernetes on windows 10

[Youtube Tutorial - How to install Kubernetes on windows 10](https://youtu.be/tB9dp-E_02I)

主要介紹如何在 Windows 10 (1803) 上安裝 Kubernetes ( k8s ) ( 使用 Hyper-V ) 教學文。

先說明一下，為了避免不必要的問題，以下的操作記得請都用系統管理員執行 (admin)，

主要安裝流程是參考 [Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)，

在開始安裝之前，請先確定 VT-x or AMD-v virtualization must be enabled in your computer’s BIOS，

可參考 [before-you-begin](https://kubernetes.io/docs/tasks/tools/install-minikube/#before-you-begin)，如果你不知道怎麼開啟，請將以上文字拿去餵 google。

( 理論上，如果你有用過 docker 和 Hyper-V，應該都已經開啟了 )

不了解 docker 的人，建議先看 [Docker 基本教學 - 從無到有 Docker-Beginners-Guide](https://github.com/twtrubiks/docker-tutorial)。

接下來是 [Install a Hypervisor](https://kubernetes.io/docs/tasks/tools/install-minikube/#install-a-hypervisor) ( 因為 docker 的關係，電腦已經有安裝 Hyper-V 了 )，

所以我們就選擇使用 Hyper-V，安裝 Hyper-V 的方法這邊就不再說明了，請 google。

接著我們要安裝 [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)，windows 需要透過 Chocolatey 來安裝 kubectl，

可參考 [Install with Chocolatey on Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-chocolatey-on-windows)。

在安裝 kubectl 之前，請先安裝 chocolatey，安裝方法可參考 [install-with-powershellexe](https://chocolatey.org/docs/installation#install-with-powershellexe)，

這邊我們使用 PowerShell 安裝，先把 PowerShell 開啟 ( 記得使用系統管理員啟動 )，執行以下指令，

```cmd
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

![alt tag](https://i.imgur.com/tNzANIY.png)

接著可以在 terminal 上輸入 `choco` 確認是否安裝正確，

![alt tag](https://i.imgur.com/f2CevpA.png)

確認安裝完 chocolatey 之後，就可以使用以下指令安裝 kubernetes-cli，

```cmd
choco install kubernetes-cli
```

他會請你確認，輸入 `y` 就對了，

![alt tag](https://i.imgur.com/ShDzGnv.png)

可在 terminal 上輸入 ` kubectl version` 確認是否安裝正確，

![alt tag](https://i.imgur.com/4BQkAxP.png)

kubectl 安裝好了之後，接著就是安裝 Minikube。

直接到 [minikube](https://github.com/kubernetes/minikube/releases) 這邊下載 `minikube-windows-amd64`，

下載下來的檔名是 `minikube-windows-amd64.exe`，依照文件的說明，我們將檔名修改為 `minikube.exe`，

然後直接把 minikube.exe 移動到 `C:\Windows\System32` 目錄底下，

![alt tag](https://i.imgur.com/HyFYobr.png)

這樣的話，直接在 terminal 上輸入 `minikube` 就可以抓到了，如下圖

![alt tag](https://i.imgur.com/IpoeRU9.png)

也可以輸入 `minikube version` 查看目前版本，

![alt tag](https://i.imgur.com/XsCYyEI.png)

***這邊特別注意，windows用戶非常建議安裝 v0.27.0 的版本 ( 原因後面說明 )***

接著是執行 `minikube start`，在執行這個指令之前，需要先做一些事情，

首先，要先建立 external network switch，可參考 [document](https://docs.docker.com/machine/drivers/hyper-v/#example)，

![alt tag](https://i.imgur.com/R9Z8Z1n.png)

建立一個外部的 Primary Virtual Switch ( 這名稱你可以隨便設定 )，

![alt tag](https://i.imgur.com/prRHNJ0.png)

因為我們是使用 Hyper-V，所以不能只使用 `minikube start`，必須要再加上 driver，可參考 [driver](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperv-driver)，

所以我們將指令修改成如下，

```cmd
minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"
```

以上需要一點時間，請耐心等待。 ( 只要沒跳出 error，就是耐心等 )。

當執行以上的指令的時候，有時候會一直遇到錯誤，如下，

```text
Starting local Kubernetes v1.10.0 cluster...
Starting VM...
Downloading Minikube ISO
 27.81 MB / 170.78 MB [=======>------------------------------------]  16.28% 26s
E1019 11:19:47.772458    1600 start.go:168] Error starting host: Error attempting to cache minikube ISO from URL: Error downloading Minikube ISO: failed to download: failed to download to temp file: failed to copy contents: local error: tls: bad record MAC.

 Retrying.
E1019 11:19:47.774463    1600 start.go:174] Error starting host:  Error attempting to cache minikube ISO from URL: Error downloading Minikube ISO: failed to download: failed to download to temp file: failed to copy contents: local error: tls: bad record MAC
================================================================================
An error has occurred. Would you like to opt in to sending anonymized crash
information to minikube to help prevent future errors?
To opt out of these messages, run the command:
        minikube config set WantReportErrorPrompt false
================================================================================
Please enter your response [Y/n]:
```

或是

```text
Error updating cluster:  downloading binaries: downloading kubelet: Error downloading kubelet v1.10.0: failed to download: failed to download to temp file: failed to copy contents: local error: tls: bad record MAC
```

如果遇到類似的錯誤，可以執行

```cmd
minikube stop
minikube delete
```

然後再刪除 `.minikube` 這個資料夾，多重複幾次就可以成功了。

這個資料夾通常會在 `C:\Users\username` 這裡，

![alt tag](https://i.imgur.com/G4w4EWt.png)

然後再重複 `minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"` 這個指令，

相關的 issues，可參考 [issues/1793](https://github.com/kubernetes/minikube/issues/1793)，

網路上的說法是說網路的問題，我想可能是 vm 裡面的網路連 server 端比較慢才導致的。

另外當你使用 minikube ( v0.30.0 ) 時， 執行 `minikube stop` 後，你會發現它一直卡在那邊，

然後如果再次執行，就會出現 error，因為這時候 Hyper-V 的 vm 已經被 lock 住了，

雖然可以使用 PowerShell 去強制關閉他，可參考 [Stop-VM](https://docs.microsoft.com/en-us/powershell/module/hyper-v/stop-vm?view=win10-ps)，

```cmd
Stop-VM -Name VM1 -Force
```

例如，`Stop-VM -Name minikube -Force`，不過我嘗試過，使用這指令也會 error，

這時候就只有等而已 ( 等到可以把這個 vm 關閉 ) :scream::sob:

相關 [issues](https://github.com/kubernetes/minikube/issues/2914)，網路上的解法是說可以使用以下指令來關閉，

```cmd
minikube ssh "sudo poweroff"
```

然後再去 Hyper-v 中 close vm 。

不過經過我的實測，我認為比較有效的解法還是將 minikube 改使用 v0.27.0 的版本比較快。

( 所以前面我才非常建議大家安裝 minikube v0.27.0 )

修改成 minikube v0.27.0 之後，執行 `minikube stop` 就都很快速了，所以 windowns 1803 用戶非常建議安裝 v0.27.0。

如果執行 `minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"` 指令正確無誤，

也沒發生任何錯誤，輸出結果會像下圖這樣

![alt tag](https://i.imgur.com/Xya4ARZ.png)

可以輸入 `minikube status` 確認，

![alt tag](https://i.imgur.com/X49LCi8.png)

正常來說，應該都要顯示 Running 。

那接著就可以執行範例的 Quickstart 了，

```cmd
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```

```cmd
kubectl expose deployment hello-minikube --type=NodePort
```

![alt tag](https://i.imgur.com/oCkntnR.png)

```cmd
# We have now launched an echoserver pod but we have to wait until the pod is up before curling/accessing it
# via the exposed service.
# To check whether the pod is up and running we can use the following:
kubectl get pod
```

```cmd
minikube service hello-minikube --url
```

![alt tag](https://i.imgur.com/qbYKrPY.png)

記得要確認 STATUS 必須是 Running 的狀態。可以嘗試瀏覽 url，如下圖，

![alt tag](https://i.imgur.com/TftZk0s.png)

## 執行環境

* Win 10 Pro 1803

## Reference

* [Install Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* [Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)

## Donation

文章都是我自己研究內化後原創，如果有幫助到您，也想鼓勵我的話，歡迎請我喝一杯咖啡:laughing:

![alt tag](https://i.imgur.com/LRct9xa.png)

[贊助者付款](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## License

MIT license
