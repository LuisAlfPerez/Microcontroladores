# Microcontroladores
# Proyecto Final

El objetivo de esta práctica es diseñar un juego en el que a través de un potenciómetro se controla la posición de un objeto. El usuario lo puede mover entre las tres líneas y el objetivo será interactuar con el escenario propuesto. Las condiciones del juego son las siguientes: 
* El juego se mantendrá en funcóon por 1 minuto y 30 segundos
* Por cada segundo que transcurra del juego, el usuario gana 1 punto. 
* En caso de caer en un obstáculo se le resta 1 punto, por cada 15 segundos de juego, el movimiento del escenario incrementa su velocidad. 
* De igual forma, el factor multiplicativo de velocidad se usa para definir los puntos al jugador. Por
ejemplo, para el tiempo de juego entre 15 y 29 segundos, los puntos al jugador por cada segundo son +2
y al caer en un obstáculo son -2; del tiempo entre los 30 y 44 segundos, los puntos por cada segundo de
juego son +3 y al caer en un obstáculo son -3 y así sucesivamente.
* Los puntos que va acumulando el jugador se muestran en la pantalla LCD y se mostrará en la extrema
derecha inferior, el puntaje más alto obtenido
* El potenciómetro para la manipulación del objeto en el juego, debe de ser capaz de girar los 270°
* Al momento de comenzar el juego se reproduce una melodía. De igual manera que la velocidad del
escenario, el factor multiplicativo de la tabla 1 se emplea para la velocidad de reproducci´on de la melodía.

# Esquemático
![Esquemático](https://github.com/LuisAlfPerez/Microcontroladores/blob/ProyectoFinal/Esquematico.jpeg)

## Código
### Código Main del PIC18F4550 para la Música del Videojuego
	#include "mcc.h"
	#include "stdio.h"
	#include "stdlib.h"
	#include <xlcd.h>

	void c4_do(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(478);
		PORTCbits.RC1 = 0;
		__delay_us(478);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	void d4_re(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(426);
		PORTCbits.RC1 = 0;
		__delay_us(426);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	void e4_mi(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(380);
		PORTCbits.RC1 = 0;
		__delay_us(380);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	void f4_fa(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(358);
		PORTCbits.RC1 = 0;
		__delay_us(358);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	void g4_sol(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(312);
		PORTCbits.RC1 = 0;
		__delay_us(312);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	void a4_la(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(284);
		PORTCbits.RC1 = 0;
		__delay_us(284);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	void b4_si(){
	    //TMR0_StartTimer();
	    while(counter_vel <= (6-velocidad)){
		PORTCbits.RC1 = 1;
		__delay_us(253);
		PORTCbits.RC1 = 0;
		__delay_us(253);
	    }
	    TMR1Flag = 0;
	    //TMR0_StopTimer();
	    counter_vel = 0;
	}
	unsigned char stoplargo(){
	    __delay_ms(50);
	    if(PORTBbits.RB0 ==  0)
		return 1; 
	    return 0;
	}
	void stopcorto(){
	    __delay_ms(5);   
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

	    TMR0_WriteTimer(53816);
	    //TMR0_WriteTimer(18660); //prescaler 110 1SEC
	    counter = 0;
	    while (1)
	    {
		if (PORTBbits.RB0 == 1){
		    TMR0_StartTimer();
		    Martinillo();
		}
		else{
		    TMR0_StopTimer();
		    counter=0;
		    counter2=0;
		    velocidad=1;
		}        
	    }
	}
### Código Main del PIC18F4550 para el Videojuego
	/**
	  Generated Main Source File

	  Company:
	    Microchip Technology Inc.

	  File Name:
	    main.c

	  Summary:
	    This is the main file generated using MPLAB(c) Code Configurator

	  Description:
	    This header file provides implementations for driver APIs for all modules selected in the GUI.
	    Generation Information :
		Product Revision  :  MPLAB(c) Code Configurator - 3.15.0
		Device            :  PIC18F4550
		Driver Version    :  2.00
	    The generated drivers are tested against the following:
		Compiler          :  XC8 1.35
		MPLAB             :  MPLAB X 3.20
	*/

	/*
	    (c) 2016 Microchip Technology Inc. and its subsidiaries. You may use this
	    software and any derivatives exclusively with Microchip products.

	    THIS SOFTWARE IS SUPPLIED BY MICROCHIP "AS IS". NO WARRANTIES, WHETHER
	    EXPRESS, IMPLIED OR STATUTORY, APPLY TO THIS SOFTWARE, INCLUDING ANY IMPLIED
	    WARRANTIES OF NON-INFRINGEMENT, MERCHANTABILITY, AND FITNESS FOR A
	    PARTICULAR PURPOSE, OR ITS INTERACTION WITH MICROCHIP PRODUCTS, COMBINATION
	    WITH ANY OTHER PRODUCTS, OR USE IN ANY APPLICATION.

	    IN NO EVENT WILL MICROCHIP BE LIABLE FOR ANY INDIRECT, SPECIAL, PUNITIVE,
	    INCIDENTAL OR CONSEQUENTIAL LOSS, DAMAGE, COST OR EXPENSE OF ANY KIND
	    WHATSOEVER RELATED TO THE SOFTWARE, HOWEVER CAUSED, EVEN IF MICROCHIP HAS
	    BEEN ADVISED OF THE POSSIBILITY OR THE DAMAGES ARE FORESEEABLE. TO THE
	    FULLEST EXTENT ALLOWED BY LAW, MICROCHIP'S TOTAL LIABILITY ON ALL CLAIMS IN
	    ANY WAY RELATED TO THIS SOFTWARE WILL NOT EXCEED THE AMOUNT OF FEES, IF ANY,
	    THAT YOU HAVE PAID DIRECTLY TO MICROCHIP FOR THIS SOFTWARE.

	    MICROCHIP PROVIDES THIS SOFTWARE CONDITIONALLY UPON YOUR ACCEPTANCE OF THESE
	    TERMS.
	*/

	#include "mcc.h"
	#include "stdio.h"
	#include "stdlib.h"
	#include <xlcd.h>

	/*
				 Main application
	 */

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



	    // the car is shown in the first position
	    while (BusyXLCD ());
	    SetDDRamAddr(car_position);

	    while (BusyXLCD());
	    WriteDataXLCD(car);

	    //the obstacles are shown, top row
	    for(unsigned char i=0; i < 10; i++)
	    {
		while(BusyXLCD());
		SetDDRamAddr(top_row[i]);

		while(BusyXLCD());
		WriteDataXLCD(obstacle);
	    }


	    //bottom row
	    for(unsigned char i=0; i < 10; i++)
	    {
		while(BusyXLCD());
		SetDDRamAddr(bottom_row[i]);

		while(BusyXLCD());
		WriteDataXLCD(obstacle);
	    }

	    //cursor off
	    while(BusyXLCD());
	    WriteCmdXLCD(0b00001100);


	    while (1)
	    {
		myKey = GetKey_sim();
		car_previous = car_position; //para poder borrar la posición anterior del carrito y después dibujar la nueva

		switch (myKey)
		{
		    case 'A': //DETENER JUEGO
			//SE limpia el LCD
			while (BusyXLCD());
			WriteCmdXLCD(0b00000001);

			//Se posiciona el cursor a la mitad del LCD de la parte de arriba
			while (BusyXLCD ());
			SetDDRamAddr(7);

			//Escribe GAME en la parte de arriba del LCD
			while(BusyXLCD());
			putrsXLCD("GAME");

			//Se posiciona el cursor a la mitad del LCD de la parte de abajo
			while (BusyXLCD ());
			SetDDRamAddr(71);

			//Escribe GAME en la parte de abajo del LCD
			while(BusyXLCD());
			putrsXLCD("OVER!");

			Sleep();

			break;

		    case 'B': //PAUSAR JUEGO
			while (myKey != 'C') //REANUDAR JUEGO
			{
			    //no hace nada
			}
			break;

		    case '2': //SUBIR
			if (car_position >=64) //si el carrito se encuentra en la fila de abajo
			{
			    car_previous = car_position; //la car_previous almancena la posición actual antes de subir, para así borrar ese carrito
			    car_position = car_position - 64; //el carrito va a subir, esto se logra restándole a su posición actual 64
			    real_car = real_car - 64;
			}
			break;

		    case '5': //BAJAR
			if (car_position <=16) //si el carrito se encuentra en la fila de arriba
			{
			    car_previous = car_position; //la car_previous almancena la posición actual antes de subir, para así borrar ese carrito
			    car_position = car_position + 64; //el carrito va a subir, esto se logra restándole a su posición actual 64
			    real_car = real_car + 64;
			}
			break;
		}

		//desplazamiento de obstáculos
		//__delay_us(4000);
		while(BusyXLCD());
		//WriteCmdXLCD(SHIFT_DISP_LEFT);
		WriteCmdXLCD(0b00011000);


		//Revalidar la posición del carrito
		if (car_position < 64 && car_position > 39) //si el carrito se encuentra arriba y ya pasó el último obstáculo
		{
		    real_car = 0; //variable que checa si ya colisiono el carrito con alguno de los obstáculos
				  //se reinicia la variable
		}

		if (car_position > 100) //si el carrito se encuentra abajo y ya pasó el último obstáculo
		{
		    real_car = 64; //la variable se reinicia a la primera posición de la fila de abajo
		}



	    }
	}



	/**
	 End of File
	*/

# Video
Link: [Video de Youtube](https://youtu.be/910YCigk5no)

