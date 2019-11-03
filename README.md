Command Line (Bash/Zsh)
------
把你想要的功能貼在你的 `~/.bashrc`、`~/.zshrc` 或 `~/.bash_profile` 最下面
記得 `source` 讓你剛貼近去的指令生效
<br>
<br>
## lazyclone
<table>
<tr>
<td>
  
  ```shell
  function lazyclone {
      reponame=${1##*/}
      reponame=${reponame%.git}
      git clone "$1" "$reponame";
      cd "$reponame";
  }
  ```
</td>
<td>
<img src="/images/lazyclone.png"</img>
</td>
</tr>
<tr>
<td colspan="2">

  可以把repo抓下來後自動 `cd` 進去
</td>
</tr>
</table>

<br>

## ipv4
1. 在 `/usr/local/bin` 建立了一個名為 `ip` 的檔案（沒有附檔名）

    ```shell
    sudo nano /usr/local/bin/ipv4
    ```
2. 然後把指令貼近去、儲存
3. 提供權限

    ```shell
    chmod +x /usr/local/bin/ipv4
    ```
<br>
<table>
<tr>
<td>
  
  **For Linux**
  ```shell
  ifconfig `route | grep ^default | sed "s/.* //"` |
  grep 'inet addr' | cut -d: -f2 | awk '{print $1}'
  ```
  
  **For macOS**
  ```shell
  ifconfig `route | grep ^default | sed "s/.* //"` |
  grep -w 'inet' | awk '{print $2}'
  ```
</td>
<td>
<img src="/images/ip.png"</img>
</td>
</tr>
<tr>
<td colspan="2">

  與其用 `ifconfig` 查 ip 然後再從一大堆資訊裡面挑出來，這個命令就單純只會印出你的 ip
</td>
</tr>
</table>
