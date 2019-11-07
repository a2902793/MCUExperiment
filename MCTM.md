<table>
<tr>
<td>
  1
</td>
<td>
 
  &nbsp;&nbsp;&nbsp;&nbsp;假設系統是一個 `10Hz` 的時鐘，也就是說一秒會有 10 個脈波。
</td>
<td>
<img src="images/MCTM/Clock.png"</img>
</td>
</tr>
<tr>
<td>
  2
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;有一個計數器叫 `HTCFG_MCTM_RELOAD`
  <br>
  其值等於 `5`，也就是每一個脈波就計數一次、數五次 (0、1、2、3、4) 就重新計數
</td>
<td>
<img src="images/MCTM/Count.png"</img>
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
<img src="images/MCTM/3in1.png"</img>
</td>
</tr>
<tr>
<td>
  4
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;MCTM 裡面有個很重要的變數叫 `prescalar` 是用來除頻，換句話說就是 `頻率除以某數` 來調整 Clock 一秒數幾次。
  <br>
  
  原先 Clock 一秒數 `10次`，今天你新創了一個 Clock 想要一秒數 `5次`，就把 `prescalar` 設成 `2`，新的頻率就會是 `10/prescalar次` `prescalar=2`。
</td>
<td>
<img src="images/MCTM/NewClock.png"</img>
<br>

  :mega: 注意哦～計數器一樣抱持不變，仍然是 `一個脈波數一次，數5次就重置`
</td>
</tr>
<tr>
<th rowspan="4">
  5
</th>
<td rowspan="4">
<p align="center">
比較一下兩者的不同，同樣都是
</p>
	
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`一個脈波數一次，數5次就重置`
  <br>
  <br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&thinsp;&thinsp;`系統 Clock` 數5次： `0.5秒` 
  <br>
  `prescalar=2 的 Clock` 數5次： `1秒鐘`
</td>
<th>	
系統 Clock
</th>
</tr>
<tr>
<td>
<img src="images/MCTM/Comp_OriginalClock.png"</img>
</td>
</tr>
<tr>
<th>
Prescalar=2 的Clock
</th>
</tr>
<tr>
<td>
<img src="images/MCTM/Comp_NewClock.png"</img>
</td>
</tr>
<tr>
</tr>
<tr>
<td>
  6
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;最後一個要理解的功能是 `Compare` 也就是一個比較值。這裡用 `MCTM_OutputInitStructure.Compare = HTCFG_MCTM_RELOAD * 3/5` 當範例。`HTCFG_MCTM_RELOAD` 等於 `5` 所以 5 * 3/5 = `3`。它會比較目前的計數有沒有小於3，小於則低電位、大則高電位。
</td>
<td>
<img src="images/MCTM/Compare.png"</img>
</td>
</tr>
<tr>
<td>
  7
</td>
<td>
	
  &nbsp;&nbsp;&nbsp;&nbsp;實際情況是：系統時鐘 `48MHz`
  &nbsp;&nbsp;&nbsp;&nbsp;計數器也就是 ( `HTCFG_MCTM_RELOAD` ) = 48MHz/2000 = `24000`
  &nbsp;&nbsp;&nbsp;&nbsp;`Compare` 是設成 `1/2`，也就是一半的時間 High、一半的時間 Low
</td>
<td>
<img src="images/MCTM/3in1.png"</img>
</td>
</tr>
</table>
