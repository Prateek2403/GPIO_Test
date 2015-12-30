#include <stdbool.h>
#include <stdint.h>
#include "inc/hw_memmap.h"
#include "inc/hw_types.h"
#include "inc/hw_gpio.h"
#include "driverlib/sysctl.h"
#include "driverlib/gpio.h"
#include "drivers/buttons.h"

uint32_t counter = 0, ret_button;
volatile uint32_t ui32Loop;
bool cont = true;

enum ButtonStates { UP, DOWN, PRESS, RELEASE };
 
enum ButtonStates delay_debounce(enum ButtonStates button_state) 
{        
	
    ret_button  = ButtonsPoll(0,0);

    if ((ret_button & ALL_BUTTONS) == LEFT_BUTTON)   /* if pressed     */
    {                      
        if (button_state == PRESS)
        {
            button_state = DOWN;
        } 
        if (button_state == UP)
         {
            SysCtlDelay(500);
            if ((ret_button & ALL_BUTTONS) == LEFT_BUTTON)
            {
                button_state = PRESS;
            }
        } 
    } 
    else 
    {                                 
        if (button_state == RELEASE)  /* if not pressed */
        {
            button_state = UP;
        } 
        if (button_state == DOWN)
        {
            if (!((ret_button & ALL_BUTTONS) == LEFT_BUTTON))
            {
                SysCtlDelay(500); 

                if (!((ret_button & ALL_BUTTONS) == LEFT_BUTTON))
                {
                    button_state = RELEASE;
                }
            }
        }
    }
    return button_state;
}

int main ()
{

	enum ButtonStates Bs;

    SysCtlPeripheralEnable(SYSCTL_PERIPH_GPIOF);
    SysCtlPeripheralEnable(SYSCTL_PERIPH_GPIOB);

    //
    // Check if the peripheral access is enabled.
    //
    while(!SysCtlPeripheralReady(SYSCTL_PERIPH_GPIOF))
    {
    }

    while(!SysCtlPeripheralReady(SYSCTL_PERIPH_GPIOB))
    {
    }

    //
    // Enable the GPIO pin for the LED (PF3).  Set the direction as output, and
    // enable the GPIO pin for digital function.
    //
	GPIOPinTypeGPIOOutput(GPIO_PORTF_BASE, GPIO_PIN_1);
	GPIOPinTypeGPIOOutput(GPIO_PORTF_BASE, GPIO_PIN_2);
	GPIOPinTypeGPIOOutput(GPIO_PORTF_BASE, GPIO_PIN_3);

	GPIOPinTypeGPIOOutput(GPIO_PORTB_BASE, GPIO_PIN_2);
    	GPIOPinTypeGPIOOutput(GPIO_PORTB_BASE, GPIO_PIN_5);

	ButtonsInit();

    //
    // Loop forever.
    //
    while(1)
    {

	ret_button  = ButtonsPoll(0,0);

	Bs = delay_debounce( Bs );

	if (Bs == PRESS)
	{
	  cont = true;
	  while( cont )
	  {
		   for(counter = 0; counter <= 5; counter++)
		   {		
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, GPIO_PIN_2);
			SysCtlDelay(2000000);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, 0x00);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, GPIO_PIN_5);
			SysCtlDelay(2000000);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, 0x00);
		   }

		   for(counter = 0; counter <= 5; counter++)
		   {		
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, GPIO_PIN_2);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, GPIO_PIN_5);
			SysCtlDelay(2000000);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, 0x00);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, 0x00);
			SysCtlDelay(2000000);
		   }

		   for(counter = 0; counter <= 5; counter++)
		   {		
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, GPIO_PIN_2);
			SysCtlDelay(2000000);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, 0x00);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, GPIO_PIN_5);
			SysCtlDelay(2000000);
			GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, 0x00);
		   }
		cont = false;
	   }
	}
	else
	{
		GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_2, 0x00);
		GPIOPinWrite(GPIO_PORTB_BASE, GPIO_PIN_5, 0x00);
	}

    }

}
