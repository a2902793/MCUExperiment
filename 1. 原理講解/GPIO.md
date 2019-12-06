## GPIO
<!--table of content-->
- [簡單介紹](#簡單介紹)
    1. [設定方式](#1-設定方式)
- [詳細講解](#詳細講解)

<!--/table of content-->
<img src="../images/ColoredLine.png">
<br><br>
<h2 align="center"><code>簡單介紹</code></h2>

### 1. 設定方式
通常範例程式裡面為了排版較好閱讀會將 GPIO 的設定移至成獨立的一個副函式名為 `GPIO_Configuration`。和 [CKCU 的設定](https://www.github.com/a2902793/MCU_Experiment/blob/master/1.%20原理講解/CKCU.md#詳細講解) 稍不同的是，只有少數必要的 GPIO 腳位會是原先預設啟用的其餘都是未設定的，所以一樣也是 **需要用到什麼再開什麼**。
```c
void GPIO_Configuration(void)
{
  /* 如果是要設定 LED 燈的話，一定要包含下面這兩組設定 (以下舉 C14 也就是最左邊的 LED 燈為例) */
  AFIO_GPxConfig(GPIO_PC, AFIO_PIN_14, AFIO_FUN_GPIO);
  GPIO_DirectionConfig(HT_GPIOC, AFIO_PIN_14,GPIO_DIR_OUT);
	
  /* 如果是要設定按鈕的話，一定要包含下面這四組設定 (以下舉 B12 也就是最左邊的按鈕為例) */
  AFIO_GPxConfig(GPIO_PB, AFIO_PIN_12, AFIO_FUN_GPIO);
  GPIO_DirectionConfig(HT_GPIOB, AFIO_PIN_12,GPIO_DIR_OUT);
  GPIO_PullResistorConfig(HT_GPIOB, GPIO_PIN_12, GPIO_PR_DISABLE);
  GPIO_InputConfig(HT_GPIO_B, AFIO_PIN_12, AFIO_FUN_ENABLE);
}
```

<table>
<tr>
<td>

這裡在設定時如果你是用學校的板子 ([`ESK32-30501 入門套件`](https://www.holtek.com.tw/esk32-30501) + [`ESK32-20001 擴充板`](https://www.holtek.com.tw/ESK32-20001)) 的話，腳位是已經幫你設定好的。
<h6 align="center">

|   | 左 | 中 | 右 |
|---|---|---|---|
| LED | C14 | C15 | C1 |
| 按鈕 | B12 | D1 | D2 |
<h6>
</td>
<td>
<img align="right" src="../images/3LED3Button_Pinout.png">
</td>
</tr>
</table>

<br><br><br><br>
<img src="../images/ColoredLine.png">
<br><br>
<h2 align="center"><code>詳細講解</code></h2>

