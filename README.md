# Linux 主机初始化

使用 Ansible 初始化 Debian、Ubuntu 主机：配置 APT 镜像、安装常用软件并设置时区。

- [要求](#要求)
- [使用](#使用)
- [项目结构](#项目结构)

## 要求

- 控制端：Python 3.10+、Ansible Core 2.17+、SSH 客户端。
- 被管理主机：Debian 或 Ubuntu，可通过 SSH 登录，并有 `sudo` 权限和 Python 3。
- 依赖集合：`community.general`（用于设置时区）。

## 使用

以下命令在控制端、项目根目录执行：

```bash
git clone https://github.com/itsjiaxu/ansible.git
cd ansible
python3 -m venv .venv
source .venv/bin/activate
python -m pip install 'ansible-core>=2.17'
ansible-galaxy collection install -r collections/requirements.yml
```

1. 编辑 `inventory/hosts.yml`，添加被管理主机。将 `debian` 替换为主机别名，并填写地址和 SSH 用户：

   ```yaml
   all:
     children:
       linux:
         hosts:
           debian:
             ansible_host: 192.0.2.10
             ansible_user: debian
   ```

2. 配置 SSH 密钥登录。以下命令在控制端执行，将示例用户名和地址替换为目标主机的 SSH 用户及 IP：

   ```bash
   ssh-keygen -t ed25519 -C "ansible"
   ```

   按提示保存到默认路径 `~/.ssh/id_ed25519`（如果该密钥已存在，不要覆盖；可选择其他文件名）。私钥只保留在控制端，不要上传或分享。

   使用目标主机现有的 SSH 密码登录，将公钥安装到该用户：

   ```bash
   ssh-copy-id -i ~/.ssh/id_ed25519.pub debian@192.0.2.10
   ```

   按提示输入目标主机账户密码。若目标主机不允许密码登录，请通过云平台、主机控制台或其他已有管理方式，将 `~/.ssh/id_ed25519.pub` 的内容添加到目标用户的 `~/.ssh/authorized_keys`。

   测试密钥登录：

   ```bash
   ssh -i ~/.ssh/id_ed25519 debian@192.0.2.10
   ```

   确认可登录后，在 `inventory/group_vars/all.yml` 设置 SSH 用户和私钥路径，与 inventory 中的用户名保持一致，例如：

   ```yaml
   ansible_user: debian
   ansible_ssh_private_key_file: ~/.ssh/id_ed25519
   ```

   若生成密钥时选择了其他文件名，将私钥路径改为对应文件。

3. 测试连接并执行初始化：

   ```bash
   ansible all -m ansible.builtin.ping
   ansible-playbook site.yml
   ```

   只初始化指定主机时，使用 `ansible-playbook site.yml --limit debian`。执行会修改目标机的 APT 源、安装软件包并设置时区。

## 项目结构

```text
.
├── ansible.cfg                         # 默认 inventory 和连接配置
├── site.yml                            # 初始化入口 playbook
├── inventory/
│   ├── hosts.yml                       # 被管理主机清单
│   └── group_vars/all.yml              # 所有主机通用变量
└── collections/
    ├── requirements.yml                # Galaxy 外部集合依赖
    └── ansible_collections/itsjiaxu/linux/
        ├── galaxy.yml                  # 集合元数据
        └── roles/linux_init/
            ├── defaults/main.yml       # 镜像、软件包和时区默认值
            ├── tasks/                  # 配置仓库、安装软件、设置时区
            └── templates/              # Debian、Ubuntu APT 源模板
```
