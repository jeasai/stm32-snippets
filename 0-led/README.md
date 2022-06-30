LED
===

Let's build a simple led blink !

Open the STM32CubeMX, and create a new project with the right card (here STM32F401RE)
![card to select](/0-led/card_to_select.png?raw=true "Card to select")
![card to select 2](/0-led/card_to_select2.png?raw=true "Card to select 2")

The led is plugged on PA5.
![led gpio output](/0-led/led_gpio_output.png?raw=true "Set LED")

Feel free to add a user label...
![user label](/0-led/user_label.png?raw=true "Set a user label")

Now, generate the code! Do not forget to set toolchain / IDE as Makefile.
![project manager](/0-led/project_manager.png?raw=true "Project manager configuration")

In the main.c, add:

```c
/* USER CODE BEGIN WHILE */
while (1)
{
    HAL_GPIO_WritePin(LED_GPIO_Port, LED_Pin, GPIO_PIN_SET);
    HAL_Delay(500);
    HAL_GPIO_WritePin(LED_GPIO_Port, LED_Pin, GPIO_PIN_RESET);
    HAL_Delay(500);
    /* USER CODE END WHILE */
```

In order to flash the stm:
```bash
make
st-flash write build/led.bin 0x8000000
```

If everything worked out well, you should now have a blinking led!
