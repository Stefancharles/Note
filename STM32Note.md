## 空调控制器笔记
### 函数流程图
------
>*  设置中断分组。2个抢占优先级，2个响应优先级
>* delay_init()初始化
>* USART1_Init()，初始化串口1，通用**同步和异步**收发器
>* UART4_Init()，初始化串口4，通用**异步**收发器
>* Buzzer_Init(),
>* TIM2_Init()，
>* HalBuzzerBlink()，LED闪烁
>* simpleAppInit()

-----
### simpleAppInit()
```c
	uint8_t temp=0;
	
	set_led_dispbuf(buf_led,0,8); //设置数码管初始显示
	
	printf("空调控制系统-V03-180327\r\n");
	LCD_Init();//初始化显示屏
	LCD_Clr();
	
	IIC_Init();//IIC总线初始化
	IIC1_Init();//IIC1总线初始化
	keyInit();//按键初始化
	
	//继电器初始化
	STM32_GPIOx_Init(RYLAY1_Init);
	STM32_GPIOx_Init(RYLAY2_Init);	
	
	HC595_GpioInit();//初始化74HC5959驱动I/O口
	
	/**************************************************
	*系统初始化部分
	*************************************************/
	P_FAN = OFF;//控制风扇寄存器
	P_LAMP = OFF;//控制加热灯寄存器
	
	temp=10;
	while(temp--)
	{
		if(PCF8563_Init() != 1)//初始化RTC
		delay_ms(10);//等待RTC初始化完毕
	}
	//LCD12864初始化显示内容
	LCD_Init();
	LCD_DispFullImg((uint8_t *)newLandEduLogo);	//显示新大陆logo
	LCD_WriteChineseString( 6, 16, (uint8_t *)KongTiaoKongZhiXiTong, 6);
	delay_ms(1000);
	LCD_Clr();//清屏	
	LCD_WriteChineseString( 2, 24, (uint8_t *)newLandEdu, 5);
	LCD_WriteEnglishString( 4, 20, (uint8_t *)"Newland Edu");
	delay_ms(1000);
	LCD_Clr();//清屏
	memset(buf_led,MAX_LEDCH,sizeof(buf_led));//设置数码管初始显示内容
	set_led_dispbuf(buf_led,0,8); //数码管显示初始化
	
	Remote_Init();//红外遥控接收模块初始化
    ```