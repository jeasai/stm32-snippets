Timers
======

Let's turn on the led every second !

Open and create a project.

Then, configure TIM2.
The input clock is APB1 timer clock for TIM2 (16MHz by default?), which can be seen in "Clock Configuration".

`Output clock` = `Input clock` / `(prescaler + 1)`  
`Interval between two interruptions` = `Counter period` / `Output clock`

So with an input clock of 16MHz, a prescaler of 15 and a counter period of 1.000.000, we get an int every second.

Here is the configuration :
![timer config](/2-timers/timer_config.png?raw=true "timer config")

Now, enable interrupts :
![nvic](/2-timers/nvic.png?raw=true "nvic config")

After generating the code, write your interrupt handler

```c
/* USER CODE BEGIN 0 */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
    // This will trigger every second
}
/* USER CODE END 0 */
```

Then, start the timer on the main function

```c
/* USER CODE BEGIN 2 */
HAL_TIM_Base_Start_IT(&htim2);
/* USER CODE END 2 */
```
