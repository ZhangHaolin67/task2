# 电控组第二次培训作业 #
**李坤**
***20263002846***
### 基础任务思路
使用 `HAL_GPIO_WritePin()` 函数改变引脚的高低电平状态，配合 `HAL_Delay(500)`函数实现 500ms 间隔的基础流水灯效果
### 非阻塞延时 ###
用系统计时器`HAL_GetTick`记录运行时间 每个周期切换一次状态 实现非阻塞延时
**实现原理说明：** 
1. 定义全局变量 `last_time`（记录上次切换时间）和 `led_state`（记录当前亮灯状态
2. 在 `while(1)` 主循环中，计算当前系统时间与 `last_time` 的差值
3. 只有当差值 `>= 500` 毫秒时，才触发一次状态切换，并更新 `last_time` 为当前时间
4. 这样在非换灯的 499 毫秒内，CPU 不会被卡死，可以随时去处理其他任务
**核心代码片段**
```c
uint8_t led_state = 0;  
uint32_t last_time = 0;  
while (1)  
{  
  if (HAL_GetTick()-last_time>=500)//计算时间间隔  
  {  
    last_time=HAL_GetTick();//更新计时器  
    //切换状态  
    if (led_state==0)  
    {  
      HAL_GPIO_WritePin(LED0_GPIO_Port,LED0_Pin,GPIO_PIN_RESET);  
      HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin,GPIO_PIN_SET);  
      HAL_GPIO_WritePin(LED2_GPIO_Port,LED2_Pin,GPIO_PIN_SET);  
      led_state=1;  
    }  
    else if (led_state==1)  
    {  
      HAL_GPIO_WritePin(LED0_GPIO_Port,LED0_Pin,GPIO_PIN_SET);  
      HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin,GPIO_PIN_RESET);  
      HAL_GPIO_WritePin(LED2_GPIO_Port,LED2_Pin,GPIO_PIN_SET);  
      led_state=2;  
    }  
    else  
    {  
      HAL_GPIO_WritePin(LED0_GPIO_Port,LED0_Pin,GPIO_PIN_SET);  
      HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin,GPIO_PIN_SET);  
      HAL_GPIO_WritePin(LED2_GPIO_Port,LED2_Pin,GPIO_PIN_RESET);  
      led_state=0;  
    }  
  }  
}
```
