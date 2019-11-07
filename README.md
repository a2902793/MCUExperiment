# 微處理機實驗考試
這個教學只適用於108學年第1學期微處理機實驗課的期中考，所使用的板子型號是HT32F52352。
<br>
<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `程式碼歸盛群半導體股份有限公司（Holtek）所有。`

<br>
<br>

## &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `前情提要`

###  1. 準備環境
a) 參考第一週投影片設置開發板的燒錄設定，確保以下設定都有做到，不然燒錄可能成功但板子不會有反應
* `Configure Flash Tools...` > `C/C++` > `Optimization`：下拉選單選取 `Level 0 (-O0)`
* `Configure Flash Tools...` > `Debug` > `Use`：確認為 `CMSIS-DAP Debugger`
* `Configure Flash Tools...` > `Debug` > `Settings` > `Flash Download`：`Reset and Run` 是打勾的

b) 再依照題目選一範例進行修改
<table>
<tr>
<td>
  
  所有程式碼都改自官方提供的範例程式，不同範例存在不同資料夾內。基本上都存在下面的路徑內，實際路徑可能有些許不同，但都是在 `C:\Holtek\` 資料夾內。
</td>
<td>
<img src="/images/Intro.gif"</img>
</td>
</tr>
<tr>
<td colspan="2">

  `C:\Holtek\HT32_STD_5xxxx_FWLib_v011_4188\example`
</td>
</tr>
</table>

c) 點進所想要使用的範例程式後，執行 `_CreateProject.bat`，它會自動幫你生成所需要的專案和程式碼。

<table cellspacing="12">
<tr>
<td td colspan=4>
  
  這裡以 `GPIO` 的 `InputOutput` 為例（如果只需要控制按鈕跟LED，修改這個範例就可以了），等執行完後會生出很多檔案和資料夾，進到 `MDK_ARMv5` 資料夾並執行 `Project_52352.uvprojx` ，演示和路徑如下。
</td>
<td td colspan=8>
<img src="/images/CreateProject.gif"</img>
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
|GPIO|General Purpose Input/Output (GPIO) 代表通用通用型之輸入輸出，功能是基本高低電位的輸入輸出|
|MCTM|Motor Control Timer (MCTM) 代表馬達控制計時器，可用於多種用途，包括通用計時、測量輸入信號脈衝寬度或產生輸出波形，如單脈衝或 PWM 輸出。|

<br>

### 4. 基本程式碼講解
有些程式碼是開發版的基本設定，基本上每個自動生出來的範例裡面都有。基本上呢 ... 這些不會是你需要動到，也不太需要了解的（如果只是想應付這次考試的話可以直接跳到考題講解:smirk:）<br>
也就是 `GPIO_PC, AFIO_PIN_1` 。往後的腳位都是用這個方法設定，也就是 GPIO 在前、AFIO 在後，為了講解方便 GPIO_P`C`, AFIO_PIN_`1`就簡稱 `C1`。<br>
<br>
<br>
<br>

## &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `考題講解`

<br>
<br>

## 第 1 題

<table>
<tr>
<td>

  ### 題目：當持續按著按鍵，執行閃爍燈（1秒亮、2秒暗），放開後停止閃爍燈。
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:no_entry_sign:嚴禁使用for迴圈delay
</td>
</tr>
</table>

<br>

### 思考步驟：
##### 1. 需要用到什麼元件？
`LED燈` `按鈕` <br>
##### 2. 怎麼用？
這題選用助教給的 `期中考提供資料包` > `實驗06：計時器—GPTM_MCTM` > `範例` > `MCTM` > `呼吸燈` > `main.c` 範例。

<table>	
<tr>
<td colspan="2">
<p align="center">第一步、設定 CKCU</p>
</td>
</tr>
<tr>
<td>
 
  &nbsp;&nbsp;&nbsp;&nbsp;LED 及 MCTM 所需要的時鐘已設定好，尚缺按鈕所需要用的，第一顆按鈕為 `B12` 所以在 `GPIO_Configuration(void)` 裡面添加啟用 Port B 的時鐘。
</td>
<td>
  
 ```diff
 void CKCU_Configuration(void)
 {
     CKCU_PeripClockConfig_TypeDef CKCUClock = {{ 0 }};
     CKCUClock.Bit.AFIO       = 1;
     CKCUClock.Bit.MCTM0      = 1;
 +   CKCUClock.Bit.PB 	     = 1;
     CKCU_PeripClockConfig(CKCUClock, ENABLE);
 }
 ```
</td>
</tr>
</table>

<table>
<tr>
<td colspan="2">
<p align="center">第二步、設定 GPIO</p>
</td>
</tr>
<tr>
<td>
 
  &nbsp;&nbsp;&nbsp;&nbsp;裡面已經設定好 `LED` 也就是 `C1`、下面 `C4` 腳位是在設定 MCTM 的中斷不要刪掉，刪了就沒有時鐘燈也就不會閃爍。
  <br>
  <br>
  &nbsp;&nbsp;&nbsp;&nbsp;因為要用到按鈕，所以還要增加按鈕相關的設定。從第三周的 PPT 裡面可以知道，最少需要增加4項設定也就是 `AFIO_GPxConfig`、`GPIO_DirectionConfig`、`GPIO_PullResistorConfig`、`GPIO_InputConfig`。
</td>
<td>
  
 ```diff
 void GPIO_Configuration(void)
 {
     /* Configure MCTM Channel 0 output pin */
     AFIO_GPxConfig(GPIO_PC, AFIO_PIN_1, AFIO_FUN_MCTM_GPTM);

     /* Configure MCTM Break pin */
     AFIO_GPxConfig(GPIO_PB, AFIO_PIN_4, AFIO_FUN_MCTM_GPTM);
	
 +   AFIO_GPxConfig(GPIO_PB, AFIO_PIN_12, AFIO_FUN_GPIO);
 +   GPIO_DirectionConfig(HT_GPIOB, GPIO_PIN_12, GPIO_DIR_IN);
 +   GPIO_PullResistorConfig(HT_GPIOB, GPIO_PIN_12, GPIO_PR_DISABLE);
 +   GPIO_InputConfig(HT_GPIOB, GPIO_PIN_12, ENABLE);
 }
 ```
</td>
</tr>
<tr>
<td colspan="3">
  <p align="center">:mega:設定腳位的API可以參考第三周投影片第20頁</p>
</td>
</tr>
</table>

<table>	
<tr>
<td colspan="3">
<p align="center">第三步、調整 MCTM</p>
</td>
</tr>
<tr>
<td>
  1
</td>
<td>
 
  &nbsp;&nbsp;&nbsp;&nbsp;假設系統是一個 `10Hz` 的時鐘，也就是說一秒會有 10 個脈波。
</td>
<td>
<img src="/images/MCTM/Clock.png"</img>
</td>
</tr>
<tr>
<td>
  2
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;又有一個計數器，每一個脈波就計數一次，數五次 (0、1、2、3、4) 就重置也就是重新計數
</td>
<td>
<img src="/images/MCTM/Reset.png"</img>
</td>
</tr>
<tr>
<td>
  3
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;所以系統時鐘、計數器、重置的情況疊在一起像這樣
</td>
<td>
<img src="/images/MCTM/3in1.png"</img>
</td>
</tr>
<tr>
<td>
  3
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;今天你新創了一個時鐘，原先一秒數 `10次` 被你換成一秒數 `5次`，也就是 `10/prescalar次` `prescalar=2`，這時候計數器仍然是 `一個脈波數一次，數5次就重置`
</td>
<td>
<img src="/images/MCTM/NewClock.png"</img>
</td>
</tr>
<tr>
<th rowspan="2">
  4
</th>
<th colspan="2">

  &nbsp;&nbsp;&nbsp;&nbsp;比較一下兩者的不同，一樣式 `一個脈波數一次，數5次就重置`，但系統時鐘因為數5次只要 `0.5秒` 而你用 `prescalar=2` 創出來的新 Clock 數五次需要 `1秒鐘`。
</th>
</tr>
<tr>
<td>
<img src="/images/MCTM/Comp_OriginalClock.png"</img>
</td>
<td>
<img src="/images/MCTM/Comp_NewClock.png"</img>
</td>
</tr>
</table>

##### 3. 在main裡寫邏輯

<br>

