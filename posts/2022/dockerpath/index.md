# 修改Docker的映像檔路徑


兩年前有學到 Docker。

<!--more-->

結果到目前為止，只有當下和學完那個月有摸到。

當時最讓人頭痛的，就是它都把資料塞在 C 槽而且無法從設定移動。  
後來找半天才讓我找到解法。

> By default,  
> Docker Desktop for Window will create 2 distros below  
> `docker-desktop`  
> `docker-desktop-data`  
> If we access the path `%LOCALAPPDATA%/Docker/wsl`  
> we can see 2 folders; and inside it is vhdx file.

> For detail,  
> `data/ext4.vhdx` which is consumed by `docker-desktop-data`  
> `distro/ext4.vhdx` which is consumed by `docker-desktop`  
> In which, docker-desktop-data is used to store images and so on.

> Therefore,  
> its size will be increased in the future, consequently,
> our System Drive will be out of space.
> Below are step-by-step to move docker-desktop-data out of System >Drive,
> for example, E:\docker-desktop\data.

> ### Step 1: Stop Docker
>
> ### Step 2: Export, unregister then import distro
>
> **1- Shutdown all WSL distros**  
> `wsl --shutdown`
>
> **2- Export docker-desktop-data to tar file**  
> `wsl --export docker-desktop-data D:\Docker\docker-desktop-data.tar`
>
> **3- Unregister current docker-desktop-data distro**  
> `wsl --unregister docker-desktop-data`
>
> **4- Import docker-desktop-data distro from tar file**  
> `wsl --import docker-desktop-data D:\Docker\data D:\Docker\docker-desktop-data.tar --version 2`
>
> _Notes:  
> In this step, we may meet the error of cannot create a specific network.  
> Just re-run the import command._
>
> ### Step 3: Start Docker
>
> `C:\ProgramData\Docker\config\daemon.json` is for windows >containers,  
> this is the default location in windows daemon code.
>
> `C:\Users\<username>\.docker\daemon.json` is for linux containers,  
> this hyper-v/wls2/linux native default location
>
> 參數為 data-root

雖不知現在的 Docker 變得如何，也不知還有沒有機會玩。

不過怕自己遇到還要找一次，還是做個記錄吧！

