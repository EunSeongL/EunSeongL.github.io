---
title: "[ex46]"
tags:
    - Study
    - Language
date: "2025-03-18"
thumbnail: "/assets/img/AI/neural.png"
bookmark: true
---
# 문제 설명
---


# 정답 코드
---

```c
//stm32f10x_it.c
volatile int Uart1_Rx_In = 0;
volatile int Uart1_Rx_Data = 0;

void USART1_IRQHandler(void)
{
   Uart1_Rx_Data = (unsigned char)USART1->DR;
  Uart1_Send_Byte(Uart1_Rx_Data);

   Uart1_Rx_In = 1;
   NVIC_ClearPendingIRQ(37);
}

volatile int note_end = 0;

void TIM2_IRQHandler(void)
{
  Macro_Clear_Bit(TIM2->SR, 0);
   NVIC_ClearPendingIRQ(28);
   note_end = 1;
}

//main.c
#include "device_driver.h"

static void Sys_Init(void)
{
   Clock_Init();
   LED_Init();
   Uart_Init(115200);
   Key_Poll_Init();

   SCB->VTOR = 0x08003000;
   SCB->SHCSR = 0;
}

#define BASE  (500) //msec

enum key{C1, C1_, D1, D1_, E1, F1, F1_, G1, G1_, A1, A1_, B1, C2, C2_, D2, D2_, E2, F2, F2_, G2, G2_, A2, A2_, B2};
enum note{N16=BASE/4, N8=BASE/2, N4=BASE, N2=BASE*2, N1=BASE*4};
const int song1[][2] = { {G1,N4},{G1,N4},{E1,N8},{F1,N8},{G1,N4},{A1,N4},{A1,N4},{G1,N2},{G1,N4},{C2,N4},{E2,N4},{D2,N8},{C2,N8},{D2,N2} };

static void Buzzer_Beep(unsigned char tone, int duration)
{
   const static unsigned short tone_value[] = {261,277,293,311,329,349,369,391,415,440,466,493,523,554,587,622,659,698,739,783,830,880,932,987};

   TIM3_Out_Freq_Generation(tone_value[tone]);
   TIM2_Delay(duration);

}

extern volatile int Uart1_Rx_In;
extern volatile int Uart1_Rx_Data;
extern volatile int note_end;

void Main(void)
{
   Sys_Init();
   TIM4_Out_Init();
   TIM3_Out_Init();
   Uart1_RX_Interrupt_Enable(1);

   static int timer_run = 0;
   
   volatile int i = 0;
   Buzzer_Beep(song1[i][0], song1[i][1]);

   for(;;)
   {
      if (Uart1_Rx_In)
      {
         Uart1_Rx_Data = Uart1_Rx_Data - '0';
         if (Uart1_Rx_Data == 0)
         {
            TIM4_Out_Stop();
            timer_run = 0;
         }
         else
         {
            if (timer_run == 0)
            {
               TIM4_Out_PWM_Generation(1000, Uart1_Rx_Data);
               timer_run = 1;
            }
            else
            {
               TIM4_Change_Duty(Uart1_Rx_Data);
            }
         }
         Uart1_Rx_In = 0;
      }

      if (note_end)
      {

         TIM3_Out_Stop();
         i++;

         if (i >= sizeof(song1)/sizeof(song1[0]))
         {
            i = 0;
         }
         Buzzer_Beep(song1[i][0], song1[i][1]);

         note_end = 0;
      }
   }
}
```

# 메모
---
> printf 내부의 **\n** 습관화 필요