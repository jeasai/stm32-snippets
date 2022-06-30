Button
======

Let's turn on the led on a button press!

Open and create a project.

The button is at PC13.
![button](/1-button/button.png?raw=true "Button")
![button 2](/1-button/button_config.png?raw=true "Button 2")

Under nvic, activate "EXTI line[15:10] interrupts
![NVIC](/1-button/nvic_cfg.png?raw=true "Button 2")

Then, generate the code.

On the main.c:
```c
/* USER CODE BEGIN 0 */
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    if (HAL_GPIO_ReadPin(Button_GPIO_Port, Button_Pin) == GPIO_PIN_RESET)
    {
        HAL_GPIO_WritePin(LED_GPIO_Port, LED_Pin, GPIO_PIN_SET);
    }
    else
    {
        HAL_GPIO_WritePin(LED_GPIO_Port, LED_Pin, GPIO_PIN_RESET);
    }
}
/* USER CODE END 0 */
```

Now compile, flash and press the button !
