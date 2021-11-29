# Microcontroladores
# Proyecto Final

El objetivo de esta práctica es decodificar un número de 8 bits que será mostrado de manera gráfica en un display de 7 segmentos, por medio de un microcontrolador PIC 18f4550, en lenguaje ensamblador. 

## Explicación del Código.

Para lograr la práctica se debieron de considerar los siguientes pasos:
1. Dividir el puerto B en 2 para realizar la suma de ambos registros de 4 bits (0-31).
2. Sumar ambos registros
3. Decodificarlo para el display de 7seg.

### Paso 1: Dividir Puerto B
Para ello, el primer paso de todo el programa, es definir nuestros puertos de E/S

  	SETF	TRISB	    ;THE WHOLE PORT B IS AN INPUT
  	CLRF	TRISD	    ;THE WHOLE PORT D IS AN OUTPUT
Después, para la división se crearon dos máscaras.
* Máscara de MSB's ("11110000")
* Máscara de LSB's ("00001111")
Con ellas podemos extraer ambos números independientes de 4 bits. Para poder utilizar los bits más significativo es necesaria la función SWAPF, la cual envía esos 4 MSB's hacía los 4 LSB's y los guarda en el acumulador.

  		MOVF	PORTB, W	
  		ANDLW	MASK2  ;00001111		
  		MOVWF	DATA_A
		
  		MOVF	PORTB, W	
  		ANDLW	MASK   ;11110000
  		MOVWF	DATA_B
  		SWAPF	DATA_B, W
		
  		ADDWF	DATA_A, W
  		MOVWF	RESULT
### Paso 2: Sumar ambos registros
Siendo esto la principal operación a realizar, esta nada más se logra por medio de la suma del acumulador, siendo los MSB's, con los 4 LSB's que fueron guardados la variable DATA_A. Este resultado de la suma es almacenado en la variable RESULT la cual será utilizada en el proceso de decodificación.
  		ADDWF	DATA_A, W
  		MOVWF	RESULT
### Paso 3: Decodificación.
Ese consiste en un switch el cual recibe el resultado de la operación y los convierte en código para el display de 7 segmentos.
![Display de 7 segmentos](https://controlautomaticoeducacion.com/wp-content/uploads/catodo.png)
Para realizar un switch, se toma el valor del resultado y se decrementa en 1. Este se va recorriendo por todos los números. Hsta que esta resta de igual a 0, se procederá a brincar al número esperado.

## Funciones

* MOVLW
> Mueve una constante al acumulador.
* MOVWF
> Mueve el contenido del acumulador a un registro.
* SETF
> Cambia todos los bits de un registro a "1". En este caso, se utiliza para definir un registro como entrada.
* CLRF
> Cambia todos los bits de un registro a "0". En este caso, se utiliza para definir un registro como salida.
* DECF
> Decrementa el contenido de un registro -1
* BZ
> En caso de que el contenido en el acumulador sea 0, se procederá a brincar a la dirección señalada.
* CALL
> Esta línea hace un brinco o llamado a otra dirección para posteriormente regresar por medio de la instrucción RETURN.
* GOTO
> Hace un brinco a la dirección señalada.
* ADDWF
> Suma el contenido del acumulador con el contenido del registro para guardarlo en cualquiera de los dos.
* SWAPF
> Hace un inversión en los bits del registro haciendo los 4 LSB's los MSB's y viceversa. 
* ANDLW
> Realiza la operación AND entre una constante y el contenido del acumulador.

# Esquemático
![Esquemático](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica1/EsquematicoP1.PNG)

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

