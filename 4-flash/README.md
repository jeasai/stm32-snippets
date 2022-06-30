FLASH
=====

Let's write data on the flash !
Create a new project with an UART.

Then, modify the linker script

```
MEMORY
{
RAM (xrw)      : ORIGIN = 0x20000000, LENGTH = 96K
ISR (xrw)      : ORIGIN = 0x08000000, LENGTH = 16K
SECTOR1 (rx)   : ORIGIN = 0x08004000, LENGTH = 16K
FLASH (wrx)    : ORIGIN = 0x08008000, LENGTH = 480K
}
```

```
    .isr_vector :
    {
    . = ALIGN(4);
    KEEP(*(.isr_vector)) /* Startup code */
    . = ALIGN(4);
    } > ISR

    .sector1_content :
    {
    *(.sector1)
    *(.sector1*)
    } > SECTOR1
```

Afterwards, let's create a variable in order to store something...

```c
/* USER CODE BEGIN 0 */
__attribute__((__section__(".sector1"))) unsigned int count;
/* USER CODE END 0 */
```

```c
  /* USER CODE BEGIN WHILE */
  char buffer[] = "  \r\n";
  while (1)
  {
    unsigned int value = count;
    buffer[0] = '0' + ((value / 10) % 10);
    buffer[1] = '0' + (value % 10);
    HAL_UART_Transmit(&huart2, buffer, 6, 200);

    HAL_FLASH_Unlock();
    FLASH_Erase_Sector(1, FLASH_VOLTAGE_RANGE_3);
    if (count > 99) {
        HAL_FLASH_Program(FLASH_TYPEPROGRAM_WORD, &count, 0);
    } else {
        HAL_FLASH_Program(FLASH_TYPEPROGRAM_WORD, &count, count + 1);
    }
    HAL_FLASH_Lock();

    HAL_Delay(2000);
    /* USER CODE END WHILE */
```

This sends an incrementing number every two seconds.  
After powering off and on the card, the count should not restart from 0.
