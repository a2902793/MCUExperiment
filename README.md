<p align="center">
<a href="https://github.com/a2902793/MCU_Experiment/blob/master/Logo-Disclaimer"><img alt="Logo Disclaimer" src="images/Logo.png" width="375" height="150"/></a>
</p>
<h1 align="center">@TKUEE / 微處理機實驗</h1>
<p align="center">
<a href="https://github.com/a2902793/MCU_Experiment/issues"><img alt="Issues" src="https://img.shields.io/github/issues/a2902793/MCU_Experiment.svg" height="20"/></a>
<a href="https://github.com/a2902793/MCU_Experiment/stargazers"><img alt="StarMe" src="https://img.shields.io/github/stars/a2902793/MCU_Experiment.svg" height="20"/></a>
<a href="https://github.com/a2902793/MCU_Experiment/graphs/contributors"><img alt="Contributors" src="https://img.shields.io/github/contributors/a2902793/MCU_Experiment.svg" height="20"/></a>
<a href="https://github.com/a2902793/MCU_Experiment/graphs/commit-activity"><img alt="Maintained" src="https://img.shields.io/badge/Maintained%3F-yes-green.svg" height="20"/></a>
</p>
<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;這個教學適用於淡江大學電機系三年級上學期微處理機實驗課（TETCB3B-0956）的同學以及那些想了解此塊開發板的人，其觀念 ***適用於其他微處理器*** 所以建議除了把上課內容弄懂之外，有時間的話也點進去各個功能去深入了解它們的原理與程式碼的撰寫。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;這門課使用的是盛群半導體股份有限公司（Holtek）的 [`ESK32-30501 入門套件`](https://www.holtek.com.tw/esk32-30501) 搭載 [`ESK32-20001 擴充板`](https://www.holtek.com.tw/ESK32-20001) 做練習，其中 ESK32-30501 入門套件所使用的微處理器是 HT32F52352 ，是一顆採用 ARM Cortex-M0+ 核心的微處理器，內建多組計時器、兩個 I<sup>2</sup>C 、兩個 SPI 、兩個 USART 、兩個 UART 、 一個12位元 ADC 、 USB 、 I<sup>2</sup>S 、 EBI 
等，可針對許多外部裝置進行試驗和開發原型。由於課堂時間有限，所以不會把所有的功能都教完，實際有教到的才會寫在這個教學中，詳情請看目錄。<br>
<br>
* 有問題歡迎開 issue 問我
* 內容可能有不足的也歡迎成為 contributor
* 最後如果這個教學對你有幫助的話也別忘記給個星星哦～:star:
<br>
<details>
<summary><b>題外話（點我）</b></summary>
我自己也是透過寫這個教學的過程中，知道怎麼去看datasheet、從中看懂各暫存器的功能、透過反推範例程式碼知道怎麼把較底層的位元操作包成較高階、淺顯易懂的API，於是在這裡把我日以繼夜研究一個多的月心得寫成教學，希望能幫你們省下一點時間。不得不說，其實反推別人的程式碼中可以學習到很多業界工程師寫的程式碼裡面的巧思，除了看教學外也非常鼓勵各位也能自己反推一遍。我想爾後如果遇到一個新的微處理器也不會怕了，就是一樣的流程而已：<ins>查看datasheet → 選擇要使用的功能 → 將位元運算包成API → 撰寫主要邏輯程式</ins>。總而言之，如果日後對軟硬體整合或是想當嵌入式系統工程師的同學，把這門課弄懂、學好 <b>受．益．良．多</b> 啊！
<br />
</details>
<br>
<p align="center"><code>程式碼歸盛群半導體股份有限公司（Holtek）所有。</code></p>
<br>
<p align="center"><h2>目錄</h2></p>

<!--table of content-->
- [前情提要](#前情提要)
    1. [準備環境](#1-準備環境)
    2. [思考步驟](#2-思考步驟)
    3. [範例介紹](#3-範例介紹)
    4. [基本程式碼講解](#4-基本程式碼講解)
- [原理講解](#原理講解)
    - [MCTM](#mctm)
<!--/table of content-->

<br>
<img src="images/ColoredLine.png">
<br>
<br>
<h2 align="center"><code>前情提要</code></h2>

###  1. 準備環境
a) 確認所需要的程式都已經安裝好，如果沒有的話可以參考上課PPT裡的步驟去下載檔案，我這裡也備份了 <a href="">Keil MDK</a> 和 <a href="4. 安裝檔/Holtek_F5xxx_Firmware.zip">Holtek 的範例程式</a>。<br><br>
<img align="right" width="139" height="50" src="images/BoardConnector.png">
b) 最常板子插上去沒反應的原因是：你插錯洞了！:flushed: <br>
&nbsp;&nbsp;&nbsp;&nbsp;注意一下 micro-usb 線是要插在兩側 PULL HERE 中間的那個接頭（只有這個接頭有燒錄功能） →
<br><br>
c) 開啟 `Keil uVision 5`，然後參考第一週投影片設置開發板的燒錄設定，確保以下設定都有做到，不然燒錄可能成功但板子不會有反應
* `Configure Flash Tools...` > `C/C++` > `Optimization`：下拉選單選取 `Level 0 (-O0)`
* `Configure Flash Tools...` > `Debug` > `Use`：確認為 `CMSIS-DAP Debugger`
* `Configure Flash Tools...` > `Debug` > `Settings` > `Flash Download`：`Reset and Run` 是打勾的

d) 再依照題目選一範例進行修改
<table>
<tr>
<td>
  
  所有程式碼都改自官方提供的範例程式，不同範例存在不同資料夾內。<br>
  學校的是在下面的路徑內，也可以使用你從<a href="https://www.holtek.com.tw/documents/10179/aa47cbc8-7203-4c7c-b311-3815080cbb2b">官網</a>或<a href="安裝檔/Holtek_F5xxx_Firmware.zip">我的備份</a>下載、解壓縮的資料夾。實際路徑可能有些許不同，但基本上如果沒有更改資料夾名字的話都是在 `HT32_STD_5xxxx_FWLib_v011_4188` 資料夾內，演示如右。
</td>
<td>
<img src="images/Intro.gif"/>
</td>
</tr>
<tr>
<td colspan="2">

  `C:\Holtek\HT32_STD_5xxxx_FWLib_v011_4188\example`
</td>
</tr>
</table>

e) 點進所想要使用的範例程式後，執行 `_CreateProject.bat`，它會自動幫你生成所需要的專案和程式碼。

<table cellspacing="12">
<tr>
<td td colspan=4>
  
  這裡以 `GPIO` 的 `InputOutput` 為例（拿到一個新板子可以燒錄這個範例來測試板子 LED、GPIO 是否正常），等執行完後會生出很多檔案和資料夾，進到 `MDK_ARMv5` 資料夾並執行 `Project_52352.uvprojx` ，演示和路徑如右。
</td>
<td td colspan=8>
<img src="images/CreateProject.gif"/>
</td>
</tr>
<tr>
<td colspan=12>

  `C:\Holtek\HT32_STD_5xxxx_FWLib_v011_4188\example\GPIO\InputOutput\MDK_ARMv5\Project_52352.uvprojx`
</td>
</tr>
</table>

<br>

### 2. 思考步驟
1. 需要用到什麼元件？
2. 怎麼用？
3. 在main裡寫邏輯

<br>

### 3. 範例介紹

|範例|功能|
|---|---|
|GPIO|General Purpose Input/Output (GPIO) 代表通用型之輸入輸出，功能是基本高低電位的輸入輸出|
|MCTM|Motor Control Timer (MCTM) 代表馬達控制計時器，可用於多種用途，包括通用計時、測量輸入信號脈衝寬度或產生輸出波形，如單脈衝或 PWM 輸出。|
<br>

### 4. 基本程式碼講解
有些程式碼是開發板的基本設定，基本上每個自動生出來的範例裡面都有。基本上呢 ... 這些不會是你需要動到，也不太需要了解的（如果只是想應付這次考試的話可以直接跳到考題講解:smirk:）<br>
<del>也就是 `GPIO_PC, AFIO_PIN_1` 。往後的腳位都是用這個方法設定，也就是 GPIO 在前、AFIO 在後，為了講解方便 GPIO_P`C`, AFIO_PIN_`1`就簡稱 `C1`。</del>