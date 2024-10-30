#define F_CPU 16000000UL  
#include <avr/io.h>        
#include <util/delay.h>    
#include <avr/interrupt.h>  

volatile uint8_t pattern_index = 0;  
volatile uint8_t button_pressed = 0; 

void gpio() 
{   
    DDRD = 0xfb;
    DDRB = 0x01;
    DDRD &= ~(1 << PD2);
    PORTD |= (1 << PD2);  
}

void lp(uint8_t pattern) 
{
    PORTD = (PORTD & 0x04) | (pattern & 0xfb);
    PORTB = (pattern & 0x04) >> 2; 
    _delay_ms(1000);  
}

void initial()
{
    lp(0x00);  
    _delay_ms(1000);  
}

void patterns()
{
    uint8_t pattern;  

    switch (pattern_index)
    {
        case 0:  
            lp(0xAA);  
            if (button_pressed) return;  
            lp(0x55);  
            if (button_pressed) return;  
            initial();
            break;

        case 1:  
            for (int i = 0; i < 8; i++) 
            {
                lp(1 << i);  
                if (button_pressed) return;  
            }
            initial();
            break;

        case 2:  
            for (int i = 0; i < 4; i++) 
            {
                lp((1 << i) | (1 << (7 - i)));  
                if (button_pressed) return;
            }
            initial();
            break;

        case 3:  
            for (int i = 0; i < 4; i++) 
            {
                lp((1 << i) | (1 << (7 - i)));
                if (button_pressed) return;
            }
            for (int i = 3; i >= 0; i--) 
            {
                lp((1 << i) | (1 << (7 - i)));
                if (button_pressed) return;
            }
            initial();
            break;

        case 4:  
            for (int i = 0; i < 8; i++) 
            {
                lp(1 << i);  
                if (button_pressed) return;
            }
            for (int i = 7; i >= 0; i--) 
            {
                lp(1 << i);  
                if (button_pressed) return;
            }
            initial();
            break;

        case 5:  
            pattern = 0x00;
            for (int i = 0; i < 8; i++) 
            {
                pattern |= (1 << i);  
                lp(pattern); 
                if (button_pressed) return;
            }
            for (int i = 0; i < 8; i++)
            {
                pattern &= ~(1 << i);  
                lp(pattern);  
                if (button_pressed) return;
            }
            initial();
            break;

        case 6: 
            pattern = 0x00; 
            for (int i = 0; i < 8; i++) 
            {
                pattern |= (1 << i);  
                lp(pattern); 
                if (button_pressed) return;
            }
            for (int i = 7; i >= 0; i--)
            {
                pattern &= ~(1 << i);  
                lp(pattern);  
                if (button_pressed) return;
            }
            initial();
            break;
    }
}

ISR(INT0_vect)
{
    pattern_index++;  
    if (pattern_index > 6)  
    {
        pattern_index = 0;
    }
    button_pressed = 1;  }

int main() 
{
    gpio(); 

    EICRA |= (1 << ISC01);  
    EIMSK |= (1 << INT0);   
    sei();      
  
    while (1) 
    {
        button_pressed = 0; 
        patterns();  
    }
}
