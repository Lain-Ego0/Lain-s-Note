### 一、卸载旧驱动
首先清除系统中可能残留的损坏或不兼容的 NVIDIA 驱动：  
```bash
sudo apt purge nvidia-* libnvidia-*  # 卸载所有NVIDIA相关包
sudo apt autoremove -y  # 清理残留依赖
sudo apt autoclean  # 清理缓存
```

### 二、禁用开源驱动  
Ubuntu 默认加载的 `nouveau` 驱动会与 NVIDIA 官方驱动冲突，需手动禁用：  
1. 创建黑名单配置文件：  
   ```bash
   sudo nano /etc/modprobe.d/blacklist-nouveau.conf
   ```
2. 在文件中添加以下内容：  
   ```plain
   blacklist nouveau
   options nouveau modeset=0
   ```
3. 更新内核模块配置：  
   ```bash
   sudo update-initramfs -u
   ```
4. 重启电脑（确保禁用生效）：  
   ```bash
   sudo reboot
   ```

### 三、安装匹配的 NVIDIA 驱动  
重启后安装适配 RTX 2080 Ti 的驱动（推荐 530 及以上版本，支持 CUDA 12.1）：  
1. 查看系统推荐的驱动版本：  

   ```bash
   sudo ubuntu-drivers devices
   ```
2. 安装推荐的驱动：  

   ```bash
   sudo ubuntu-drivers autoinstall  # 自动安装推荐版本
   ```
3. 再次重启电脑（驱动需重启加载）：  
   ```bash
   sudo reboot
   ```

### 四、验证驱动是否正常  
重启后运行以下命令确认驱动工作：  
```bash
nvidia-smi
```
若输出包含 RTX 2080 Ti 显卡信息、驱动版本及 CUDA 版本，说明驱动安装成功。

### 五、确认 PyTorch 调用 CUDA  
激活conda虚拟环境环境后验证：  
```bash
conda activate lain
python -c "import torch; print('CUDA 可用:', torch.cuda.is_available())"
```
正常应显示 `CUDA 可用: True`。

### 关键注意事项  
- **安全启动（Secure Boot）**：若驱动安装后失效，进入 BIOS（开机按 Del/F2），将「Secure Boot」设为「Disabled」（禁用），保存重启（安全启动会阻止未签名驱动加载）。  

- **驱动版本**：RTX 2080 Ti 支持最新 NVIDIA 驱动，直接安装系统推荐的高版本即可。

按以上步骤操作后，通常能解决驱动通信问题，让 PyTorch 正常使用 GPU 加速。
![1280X1280](./Pytorch-CUDA可用False 解决方案.assets/1.PNG)
