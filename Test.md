```c
int main(void)
{
  u32 i, init;
  CKCU_configuration();
  USART_configuration();
  GPIO_configuration();

  NVIC_EnableIRQ(USART1_IRQn);

  init = SPI_FLASH_Init(); //初始化 SPI

  while(init)
  {
    FLASH_ID = SPI_FLASH_ReadJEDECID(); //讀取 Flash 的 ID
    SPI_FLASH_WriteStatus(0x00); //清除保護位元
    SPI_FLASH_SectorErase(FLASH_SectorToErase); //清除想要要寫入的磁區
    SPI_FLASH_BufferWrite(Tx_Buffer, FLASH_WriteAddress, BufferSize); //將 Tx-Buffer 裡的資料寫進 Flash

    if(GPIO_ReadInBit(HT_GPIOB,GPIO_PIN_12)==1)
    {
      SPI_FLASH_BufferDualRead((u16*)Rx_Buffer, FLASH_ReadAddress, BufferSize/2); //將 Flash 裡的資料以雙倍
                                                                                  //讀取模式讀到 Rx_Buffer
      for(i = 0;i<count;i++)
      {
        USART_SendData(HT_USART1,Rx_Buffer[i]); //Send Rx_Buffer Data
      }
      for(i=0;i<10000000;i++);
    }
  }
}
```
