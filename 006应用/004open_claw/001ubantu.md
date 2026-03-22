# mac链接windows
### Step1.Mac 端生成现代 SSH 密钥

在你的 MacBook Pro 上打开终端 (Zsh)，我们使用目前更安全、性能更好的 `ed25519` 算法来生成密钥。
```bash
# 1. 生成密钥（一路回车保持默认即可，不需要设置额外密码）  
ssh-keygen -t ed25519 -C "macbook-to-win"  
​  
# 2. 查看并复制生成的公钥内容（以 ssh-ed25519 开头的一长串字符）  
cat ~/.ssh/id_ed25519.pub  
```
​

**请将终端输出的这一整段公钥文本复制下来，我们马上要在 Windows 上用到它。**

---

### Step1.Windows 端“强行”注入公钥与权限修复

因为你的 Windows 账号是管理员，普通的 `~/.ssh/authorized_keys` 是无效的。Windows 要求必须写入全局的 `administrators_authorized_keys` 文件，并且对该文件的权限有着变态级别的要求。

请在你的 Windows 电脑上，**以管理员身份运行 PowerShell**，然后逐行执行以下脚本：

```bash
# 1. 确认你当前的真实系统用户名（请记住这个名字，比如 zhang，下一步 Mac 连过来时要用）  
$env:USERNAME  
​  
# 2. 将你在 Mac 上复制的公钥，替换到下面单引号内，并执行！  
$macPublicKey = '把这里替换成你刚刚在Mac上复制的 ssh-ed25519 开头的公钥文本'  
​  
# 3. 写入 Windows 管理员专属的认证文件  
$adminKeyFile = "$env:ProgramData\ssh\administrators_authorized_keys"  
if (!(Test-Path -Path $env:ProgramData\ssh)) { New-Item -Path $env:ProgramData\ssh -ItemType Directory }  
Add-Content -Path $adminKeyFile -Value $macPublicKey  
​  
# 4. 修复极其严格的文件权限（核心步骤，不执行会拒绝读取）  
icacls.exe $adminKeyFile /inheritance:r  
icacls.exe $adminKeyFile /grant "SYSTEM:(F)"  
icacls.exe $adminKeyFile /grant "BUILTIN\Administrators:(F)"  
​  
# 5. 重启 SSH 服务让配置生效  
Restart-Service sshd  
```
​

---

### 第三步：Mac 端配置优雅的“一键直连”

回到你的 MacBook Pro，作为开发者，每次敲完整的 IP 和用户名太繁琐了。我们直接在 Mac 上配置 SSH Config。

在 Mac 终端执行：
```bash

# 编辑或创建 ssh 配置文件  
vim ~/.ssh/config  
​

# 在文件中添加以下内容（注意将 `User` 后面的值，替换为你在 Windows 第二步第 1 行查到的真实用户名，比如 `zhang`）：

Host win-ai  
    HostName 100.89.121.90  
    User zhang  
    IdentityFile ~/.ssh/id_ed25519  
    Port 22  
​
```

按下 `Ctrl + O` 保存，回车确认，然后按 `Ctrl + X` 退出。

### 🚀 终极测试

现在，在你的 Mac 终端里，只需要输入极其简短的两个词：
```bash
ssh win-ai  
```


# ubantu配置
```bash
wsl --list
wsl --unregister Ubuntu-24.04
wsl --unregister Ubuntu
wsl --install -d Ubuntu 
wsl --set-default Ubuntu
```

**启动ubantu命令行**
```bash
# 初始化用户 zhang 123456

# 免密执行
echo "zhang ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/zhang
sudo chmod 0440 /etc/sudoers.d/zhang

# 更新软件源并升级自带包
sudo apt update && sudo apt upgrade -y

# 安装核心开发工具链和网络工具
sudo apt install -y \
    build-essential \
    curl \
    wget \
    git \
    software-properties-common
```
**安装 Node.js 22 (针对 OpenClaw 和前端工具)：**
```bash
# 下载nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash 
# 立即生效（无需重启终端）
export NVM_DIR="$HOME/.nvm"; [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
# 安装 Node
 nvm install 22
```

**安装 uv (极速 Python 包与环境管理器)：**

```
curl -LsSf https://astral.sh/uv/install.sh | sh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc

# 刷新环境变量使所有配置立即生效
source ~/.bashrc
```

Tailscale  暂时不用安装
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
# WSL2 和 Tailscale 有时会有网络包大小冲突导致连接挂起，执行以下命令优化：
sudo ip link set dev eth0 mtu 1500
```

安装openclaw


修改主机名称

git
```bash
git config --global user.name "bigdragon"  
git config --global user.email "291640894@qq.com"  
  
ssh-keygen -t rsa -C "291640894@qq.com"

ssh -T git@github.com


```
ollama
```
curl -fsSL https://ollama.com/install.sh | sh


curl http://192.168.10.234:11434/api/tags

curl http://127.0.0.1:11434/api/tags
```



curl http://192.168.10.234:11434/api/tags

