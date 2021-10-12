# Microcontroladores
# Práctica 3

El objetivo es implementar un sistema basado en microcontrolador y desarrollado con el compilador XC8 que permita instrumentar un sensor de proximidad midiendo el ancho de pulso  de su señal de salida y mostrar el resultado en una pantalla LCD.

## Explicación del Código.

Para lograr la práctica se debieron de considerar los siguientes pasos:
1. Se manda pulso de la señal del ultrasónico
2. Se activa el timer para contar la duración del pulso
3. Se guarda la duración y se apaga el timer
4. Se convierte el tiempo en distancia
5. Se muestra en el LCD

### Paso 1: Pulso del sensor ultrasónico
		PORTAbits.RA0 = 1; //trigger
		__delay_us(10);
		PORTAbits.RA0 = 0;

### Paso 2-3: Timer 1
Al realizar el envío del pulso, se lee el pulso positivo para poder. En el momento que se detecta el cambio de 0 a 1, el timer 1 comienza a correr hasta que haya un cambio en el echo o el timer termine. Esta precaución se hizo tomando en cuenta si el modelo no tuviese incluido su propio tiempo de vida.

	while(PORTBbits.RB7==0); //espera a recibir un pulso positivo: echo
	TMR1_StartTimer(); //Timer empieza 
	TMR1_WriteTimer(0); //Empieza en 0
	while(PORTBbits.RB7==1 && !TMR1IF); //espera a que el pulso caiga 
	time = TMR1_ReadTimer(); //Obtiene la información del Timer
	TMR1_StopTimer(); //Detiene el timer

### Paso 4: Conversión de la señal.
Así como visto en clase, para una debida conversión a centímetros, la señal del modelo HC-SR04 debe ser dividida entre 58 o multiplicada por 0.01715. 

	dist = ((time)/29/2);

### Paso 5: Mostrar en LCD
Simplemente, si este led estaba en ON, se apagará, y viceversa. Esto como se explicó en los objetivos de la práctica, es para poder ver los periodos en un oscilador y comprobar que los ciclos sean correctos.
	
		sprintf(buffer, "Distance %.4f cm", dist);
		 while(BusyXLCD());
		    SetDDRamAddr(0x0);
		while(BusyXLCD());
		    putsXLCD((char *) buffer);

# Conclusión
En conclusión, se logró el objetivo de la práctica haciendo uso de instrucciones como nop y teniendo en cuenta el tiempo de duración de cada instrucción con el reloj usado y su configuración en los configuration bits, cabe mencionar que este proyecto pudo haberse realizado de una forma más sencilla con la implementación de algún timer y de interrupciones, ya que hasta que no hubiera un cambio de número no se podía cambiar la velocidad, sin embargo los resultados son fructíferos.



## Código
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
	    int time;
	    float dist;
	    TMR1IF = 0;

	    while (1)
	    {
		PORTAbits.RA0 = 1; //trigger
		__delay_us(10);
		PORTAbits.RA0 = 0;

		while(PORTBbits.RB7==0); //espera a recibir un pulso positivo: echo
		    TMR1_StartTimer(); //Timer empieza 
		    TMR1_WriteTimer(0); //Empieza en 0
		while(PORTBbits.RB7==1 && !TMR1IF); //espera a que el pulso caiga 
		    time = TMR1_ReadTimer(); //Obtiene la información del Timer
		    TMR1_StopTimer(); //Detiene el timer

		//dist = (time * 0.1667)/29/2;
		dist = ((time)/29/2);

		sprintf(buffer, "Distance %.4f cm", dist);
		 while(BusyXLCD());
		    SetDDRamAddr(0x0);
		while(BusyXLCD());
		    putsXLCD((char *) buffer);

		__delay_ms(250);
	    }
	}

# Implementación
Link: [Video de Youtube](https://youtube.com/shorts/-isTSOZY7c0?feature=share)
