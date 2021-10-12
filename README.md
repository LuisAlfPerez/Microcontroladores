# Microcontroladores
# Práctica 4

El objetivo es implementar un sistema basado en microcontrolador y desarrollado con el compilador XC8 que permita instrumentar un sensor de proximidad midiendo el ancho de pulso  de su señal de salida y mostrar el resultado en un arreglo de displays de 7 segmentos.

## Explicación del Código.

Para lograr la práctica se debieron de considerar los siguientes pasos:
1. Cálculo de tiempo de cada instrucción.
2. Declaración de puertos E/S
3. Decrementar la cuenta/Resetear la cuenta en caso de 0
4. Decodificarlo para el display de 7seg.
5. Cambiar el estado del LED de salida.
6. Checar cambios de velocidad
7. Cambiar velocidad y esperar el tiempo necesario.
8. Loop a partir del paso 3.

### Paso 1: Cálculo de tiempo de cada instrucción.
Tomando en cuenta un cristal de 20MHz, se hace una conversión en los bits de configuracion resultando en un reloj de 24MHz. Puesto que cada instrucción ocupa 4 ciclos, se obtienen 6M de instrucciones. Sacando el inverso, se obtiene que el tiempo por instrucción es de 166ns.

### Paso 2: Declaración de puertos E/S.
Para ello, el primer paso de todo el programa, es definir nuestros puertos de E/S

	SETF	TRISA	    ;THE WHOLE PORT A IS AN INPUT
	CLRF	TRISB	    ;THE WHOLE PORT B IS AN OUTPUT
	CLRF	TRISD	    ;THE WHOLE PORT D IS AN OUTPUT

### Paso 3: Decrementar la cuenta/Resetear la cuenta en caso de 0.
En el loop, se verifica el estado del contador. En caso de ser igual a 0, entra a una función que resetea la cuenta a 0. De lo contrario, se decrementará y seguirá al siguiente paso.

	MOVLW	0X00
	CPFSGT	COUNTER
	GOTO	CONTADOR9
	MOVLW	0X01
	CPFSLT	COUNTER
	DECF	COUNTER
	
### Paso 4: Decodificación.
Ese consiste en un switch el cual recibe el resultado de la operación y los convierte en código para el display de 7 segmentos.
![Display de 7 segmentos](https://controlautomaticoeducacion.com/wp-content/uploads/catodo.png)
Para realizar un switch, se toma el valor del resultado y se decrementa en 1. Este se va recorriendo por todos los números. Hsta que esta resta de igual a 0, se procederá a brincar al número esperado.

### Paso 5: Cambiar el estado del LED de salida.
Simplemente, si este led estaba en ON, se apagará, y viceversa. Esto como se explicó en los objetivos de la práctica, es para poder ver los periodos en un oscilador y comprobar que los ciclos sean correctos.
	
	LEDVEL:
		INCF	LED_OSC, 1
		BTFSC	LED_OSC, 0
		GOTO	LED_ON
		GOTO	LED_OFF
	LED_ON:
    		BSF	PORTB, 5
    		RETURN
	LED_OFF:
    		BCF	PORTB, 5
    		RETURN


### Paso 6: Checar cambios de velocidad
Puesto que se verifica si algún boton del puerto A, en el momento que se detecta la presión del botón, se espera a que el usuario deje de presionar este para poder interpretar el incremento o decremento de velocidad. Esto sólo es una medida de seguridad. Después, se verifica el estado actual de la velocidad: si este es igual a 5 y quiere incrementar, o si es 0 y quiere decrementar, el programa omitira la presión del botón y continuará con su misma velocidad. De lo contrario, se decrementará o incrementará según el botón que haya apretado. Se guardará el estado actual de la velocidad en una variable para poder después ser usada en un switch y cambiar la velocidad según el número de NOPs a realizar.

	VERIF_A:
    		BTFSC	PORTA, 0	
    		GOTO	VERIF_A	    	
    		MOVLW	0X04
    		CPFSGT	VELOCIDAD
    		INCF	VELOCIDAD, 1	;CHECAR NO REBASAR EL 5
    		MOVLW	0X01
    		CPFSGT	VELOCIDAD
    		GOTO	LED1
    		MOVLW	0X02
    		CPFSGT	VELOCIDAD
    		GOTO	LED2
    		MOVLW	0X03
    		CPFSGT	VELOCIDAD
    		GOTO	LED3
    		MOVLW	0X04
    		CPFSGT	VELOCIDAD
    		GOTO	LED4
    		MOVLW	0X05
    		CPFSGT	VELOCIDAD
    		GOTO	LED5
    		GOTO CHECKVEL
    
    VERIF_B:
    		BTFSC	PORTA, 1	 
    		GOTO	VERIF_B	    	
    		MOVLW	0X1
    		CPFSLT	VELOCIDAD
    		DECF	VELOCIDAD, 1	;CHECAR NO REBASAR EL 0
    		MOVLW	0X01
    		CPFSGT	VELOCIDAD
    		GOTO	LED1
    		MOVLW	0X02
    		CPFSGT	VELOCIDAD
    		GOTO	LED2
    		MOVLW	0X03
    		CPFSGT	VELOCIDAD
    		GOTO	LED3
    		MOVLW	0X04
    		CPFSGT	VELOCIDAD
    		GOTO	LED4
    		MOVLW	0X05
    		CPFSGT	VELOCIDAD
    		GOTO	LED5
    		GOTO CHECKVEL
### Paso 7: Cambiar velocidad y esperar el tiempo necesario.
La función depende de una unidad básica que definimos de 100ms, puesto que es nuestra velocidad mínima. Para ello, se separo en 4 fors anidados que nos permiten usar los Nops, de una duración de 166ns, hasta completar los 100ms. Posteriormente, se checa si el estado de la velocidad es mayor, se procede a realizar 4 veces este mismo proceso para llegar a los 500ms. En dado caso de ser mayor el estado, se realizará el proceso acumulado 5 veces más para llegar a 1s. Si el estado es mayor, se repetirá el acumulado 4 veces más para llegar a los 5s. Y finalmente, si el estado es 5, se realizara 5 veces más el proceso acumulado de 1 segundo para llegar a los 10 segundos finales. En cualquier caso, se regresará al loop al finalizar su tiempo.

## Funciones

* ADDWF
> Suma el contenido del acumulador con el contenido del registro para guardarlo en cualquiera de los dos.
* ANDLW
> Realiza la operación AND entre una constante y el contenido del acumulador.
* BCF
> Se cambia el estado de un bit especificado de un registro a '0'.
* BSF
> Se cambia el estado de un bit especificado de un registro a '1'.
* BTFSC
> Verifica el estado de un bit especificado de un registro. Si el bit está en '0', se saltará la siguiente línea. De lo contrario, seguira su flujo sin saltar.
* BZ
> En caso de que el contenido en el acumulador sea 0, se procederá a brincar a la dirección señalada.
* CALL
> Esta línea hace un brinco o llamado a otra dirección para posteriormente regresar por medio de la instrucción RETURN.
* CLRF
> Cambia todos los bits de un registro a "0". En este caso, se utiliza para definir un registro como salida.
* CPFSGT
> Compara el contenido de un registro con el del acumulador. Si el contenido del registro es mayor que el acumulador, se saltará la siguiente línea. De lo contrario, seguira su flujo sin saltar.
* CPFSLT
> Compara el contenido de un registro con el del acumulador. Si el contenido del registro es menor que el acumulador, se saltará la siguiente línea. De lo contrario, seguira su flujo sin saltar.
* DECF
> Decrementa el contenido de un registro -1
* DECFSZ
> Decrementa el contenido de un registro -1. Si el contenido del registro es igual a '0', se saltará la siguiente línea. De lo contrario, seguira su flujo sin saltar.
* GOTO
> Hace un brinco a la dirección señalada.
* INCF
> Incrementa el contenido de un registro +1
* MOVF
> Mueve el contenido de un registro al acumulador.
* MOVLW
> Mueve una constante al acumulador.
* MOVWF
> Mueve el contenido del acumulador a un registro.
* NOP
> No Operation: no se realiza ninguna operación, de esta manera se consumen ciclos de operación.
* SETF
> Cambia todos los bits de un registro a "1". En este caso, se utiliza para definir un registro como entrada.
* SWAPF
> Hace un inversión en los bits del registro haciendo los 4 LSB's los MSB's y viceversa. 

# Esquemático
![Esquemático](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica2/Esquematico.jpeg)

# Conclusión
En conclusión, se logró el objetivo de la práctica haciendo uso de instrucciones como nop y teniendo en cuenta el tiempo de duración de cada instrucción con el reloj usado y su configuración en los configuration bits, cabe mencionar que este proyecto pudo haberse realizado de una forma más sencilla con la implementación de algún timer y de interrupciones, ya que hasta que no hubiera un cambio de número no se podía cambiar la velocidad, sin embargo los resultados son fructíferos.



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

# Simulación
Link: [Video de Youtube](https://youtu.be/20Z0WnfhyOk)

# Implementación
![Implementación](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica2/Implementacion1.jpeg)
![Osciloscopio](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica2/Implementacion2.jpeg)
Link: [Video de Youtube](https://youtu.be/lYw_6ewRUtg)
