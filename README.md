微處理機實驗考試
------
這個教學只適用於108學年第1學期微處理機實驗課的期中考，所使用的板子型號是HT32F52352。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `程式碼歸盛群半導體股份有限公司（以下稱 Holtek）所有。`

<br>
<br>

## 前情提要 - 開啟所需要修改的範例專案
* 所有程式碼都改自官方提供的範例程式，不同範例存在不同資料夾內。基本上都存在下面的路徑內，實際路徑可能有些許不同，但都是在 `C:\Holtek` 資料夾內。
<table>
<tr>
<td>
  
  `C:\Holtek\HT32_STD_5xxxx_FWLib_v011_4188\example`
</td>
</tr>
<tr>
<td>
<img src="/images/Intro.gif"</img>
</td>
</tr>
</table>

<br>

* 點進所想要使用的範例程式後，執行 `_CreateProject.bat`。它會自動幫你生成所需要的專案。
這裡以 `GPIO` 的 `InputOutput` 為例（如果只需要控制按鈕跟LED，修改這個範例就可以了），等執行完後會生出很多檔案和資料夾，進到 `MDK_ARMv5` 資料夾並執行 `Project_52352.uvprojx` ，演示和路徑如下。
<table>
<tr>
<td>
  
  `C:\Holtek\HT32_STD_5xxxx_FWLib_v011_4188\example\GPIO\InputOutput\MDK_ARMv5\Project_52352.uvprojx`
</td>
</tr>
<tr>
<td>
<img src="/images/CreateProject.gif"</img>
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
