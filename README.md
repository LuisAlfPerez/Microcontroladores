# Microcontroladores
# Práctica 4

El objetivo es implementar un sistema basado en microcontrolador y desarrollado con el compilador XC8, que permita instrumentar una bocina o audífonos para reproducir 3 melodías de al menos 20s. Con la ayuda de un 3 push buttons, se seleccionará la melodía a reproducir.

## Explicación del Código.
El código funciona con base en interrupciones externas. Puesto que lo único que se hacía era variar la frecuencia de salida para cada una de las notas, se hizo una función para cada una de las notas con respecto a timer 1. Timer 0 era el encargado de hacer que sonara y dejara de sonar. Por medio de tres interrupciones se hacía el cambio de canciones, las cuales llamaban las notas a tocar, con una interrupción extra para administrar un cambio de canción durante un stop largo. Las funciones stoplargo() y stopcorto() solo son delays entre versos y notas para que las canciones sonaran bien.

	unsigned char stoplargo(){
		__delay_ms(50);
		if(Int3Flag == 1)
			return 1; 
		return 0;
	}
	
	void stopcorto(){
	    __delay_ms(5);   
	}

	void d4_re(){
		TMR0_StartTimer();
		while(TMR1Flag == 0){
			PORTCbits.RC1 = 1;
			__delay_us(426);
			PORTCbits.RC1 = 0;
			__delay_us(426);
		}
		TMR1Flag = 0;
		TMR0_StopTimer();
	}
	
	TMR0_WriteTimer(55950);
	while (1)
	{
		Int3Flag = 0;
		// Add your application code
		if(Int0Flag == 1)
		{
		    //cancion 1
		    JingleBells();
		}
		if(Int1Flag == 1)
		{
		    //cancion 2
		    MiCorazonEncantado();
		}
		if(Int2Flag == 1)
		{
		    //cancion 3
		    Martinillo();
		}
	}
	
![Notas](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica4/notas.png)


# Esquemático
![Esquemático](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica4/Practica4Esquematico.jpeg)

# Conclusión
El manejo de las interrupciones quedó completamente implementado. Tambien usar dos timers fue un reto para su debida sincronización. 



## Código
	#include "mcc.h"
	#include "stdio.h"
	#include "stdlib.h"
	#include <xlcd.h>

	void c4_do(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(478);
		PORTCbits.RC1 = 0;
		__delay_us(478);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}


	void d4_re(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(426);
		PORTCbits.RC1 = 0;
		__delay_us(426);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}

	void e4_mi(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(380);
		PORTCbits.RC1 = 0;
		__delay_us(380);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}

	void f4_fa(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(358);
		PORTCbits.RC1 = 0;
		__delay_us(358);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}

	void g4_sol(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(312);
		PORTCbits.RC1 = 0;
		__delay_us(312);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}

	void a4_la(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(284);
		PORTCbits.RC1 = 0;
		__delay_us(284);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}

	void b4_si(){
	    TMR0_StartTimer();
	    while(TMR1Flag == 0){
		PORTCbits.RC1 = 1;
		__delay_us(253);
		PORTCbits.RC1 = 0;
		__delay_us(253);
	    }
	    TMR1Flag = 0;
	    TMR0_StopTimer();
	}
	unsigned char stoplargo(){
	    __delay_ms(50);
	    if(Int3Flag == 1)
		return 1; 
	    return 0;
	}
	void stopcorto(){
	    __delay_ms(5);   
	}
	void JingleBells(){
	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	   if(stoplargo()==1)return;

	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();

	    if(stoplargo()==1)return;
	}
	void MiCorazonEncantado(){
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    b4_si();
	    stopcorto();
	    a4_la();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    f4_fa();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    b4_si();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();

	    if(stoplargo()==1)return;

	    c4_do();
	    stopcorto();
	    b4_si();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();

	    if(stoplargo()==1)return;

	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    a4_la();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();

	    if(stoplargo()==1)return;

	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();

	    if(stoplargo()==1)return;

	    d4_re();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    c4_do();

	    if(stoplargo()==1)return;

	    c4_do();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    g4_sol();

	    if(stoplargo()==1)return;
	}
	void Martinillo(){
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    c4_do();
	    stopcorto();

	    if(stoplargo()==1)return;

	    e4_mi();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();

	    if(stoplargo()==1)return;

	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    a4_la();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    f4_fa();
	    stopcorto();
	    e4_mi();
	    stopcorto();
	    c4_do();
	    stopcorto();

	    if(stoplargo()==1)return;

	    d4_re();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();
	    d4_re();
	    stopcorto();
	    g4_sol();
	    stopcorto();
	    c4_do();
	    stopcorto();

	    if(stoplargo()==1)return;
	}
	void main(void)
	{
	    // Initialize the device
	    SYSTEM_Initialize();

	    // Enable the Global Interrupts
	    INTERRUPT_GlobalInterruptEnable();

	    // Enable the Peripheral Interrupts
	    INTERRUPT_PeripheralInterruptEnable();

	    // Disable the Global Interrupts
	    //INTERRUPT_GlobalInterruptDisable();

	    // Disable the Peripheral Interrupts
	    //INTERRUPT_PeripheralInterruptDisable();

	    TMR0_WriteTimer(55950);
	    while (1)
	    {
		Int3Flag = 0;
		// Add your application code
		if(Int0Flag == 1)
		{
		    //cancion 1
		    JingleBells();
		}
		if(Int1Flag == 1)
		{
		    //cancion 2
		    MiCorazonEncantado();
		}
		if(Int2Flag == 1)
		{
		    //cancion 3
		    Martinillo();
		}

	    }
	}

# Implementación
![Esquematico](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica4/Practica4Esquematico.jpeg)

Link: [Video de Youtube](https://youtu.be/ETZb_EwNfGE)
