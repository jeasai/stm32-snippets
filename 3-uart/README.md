UART
====

Let's send what we receive with the UART !

Quick note :
`HAL_UART_Receive` => blocking  
`HAL_UART_Receive_IT` => non blocking

With polling
------------

Open and create a project

Convigure USART2 as shown here
![usart2 config](/3-uart/uart_config.png?raw=true "usart2 config")

Let's define some variables

```c
/* USER CODE BEGIN 0 */
const char *out1 = "ok\r\n";
const char *out2 = "ko\r\n";
char in[2];
/* USER CODE END 0 */
```

now, we can Receive and transmit easely !
```c
/* USER CODE BEGIN WHILE */
while (1)
{
    if (HAL_UART_Receive(&huart2, &in, 2, 1000) == HAL_OK)
    {
        if (in[0] == 'o' && in[1] == 'k')
        {
            HAL_UART_Transmit(&huart2, out1, 6, 200);
        }
        else
        {
            HAL_UART_Transmit(&huart2, out2, 6, 200);
        }
    }
    /* USER CODE END WHILE */
```

```bash
screen /dev/ttyACM1 115200
```

