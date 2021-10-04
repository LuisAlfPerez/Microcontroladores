# Microcontroladores
# Práctica 3

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
  	;*******Header Files***********
        list	    p=18f4550        ; list directive to define processor
        #include    "p18f4550.inc"  

    ;******Configuration Bits***********
    ; PIC18F4550 Configuration Bit Settings 

    ; ASM source line config statements 

    ;#include "p18F4550.inc"    

    ; CONFIG1L
      CONFIG  PLLDIV = 5            ; PLL Prescaler Selection bits (Divide by 5 (20 MHz oscillator input))
      CONFIG  CPUDIV = OSC3_PLL4    ; System Clock Postscaler Selection bits ([Primary Oscillator Src: /3][96 MHz PLL Src: /4])
      CONFIG  USBDIV = 2            ; USB Clock Selection bit (used in Full-Speed USB mode only; UCFG:FSEN = 1) (USB clock source comes from the 96 MHz PLL divided by 2)   

    ; CONFIG1H
      CONFIG  FOSC = HSPLL_HS       ; Oscillator Selection bits (HS oscillator, PLL enabled (HSPLL))
      CONFIG  FCMEN = OFF           ; Fail-Safe Clock Monitor Enable bit (Fail-Safe Clock Monitor disabled)
      CONFIG  IESO = OFF            ; Internal/External Oscillator Switchover bit (Oscillator Switchover mode disabled) 

    ; CONFIG2L
      CONFIG  PWRT = ON             ; Power-up Timer Enable bit (PWRT enabled)
      CONFIG  BOR = OFF             ; Brown-out Reset Enable bits (Brown-out Reset disabled in hardware and software)
      CONFIG  BORV = 3              ; Brown-out Reset Voltage bits (Minimum setting 2.05V)
      CONFIG  VREGEN = OFF          ; USB Voltage Regulator Enable bit (USB voltage regulator disabled) 

    ; CONFIG2H
      CONFIG  WDT = OFF             ; Watchdog Timer Enable bit (WDT disabled (control is placed on the SWDTEN bit))
      CONFIG  WDTPS = 32768         ; Watchdog Timer Postscale Select bits (1:32768)    

    ; CONFIG3H
      CONFIG  CCP2MX = OFF          ; CCP2 MUX bit (CCP2 input/output is multiplexed with RB3)
      CONFIG  PBADEN = OFF          ; PORTB A/D Enable bit (PORTB<4:0> pins are configured as digital I/O on Reset)
      CONFIG  LPT1OSC = OFF         ; Low-Power Timer 1 Oscillator Enable bit (Timer1 configured for higher power operation)
      CONFIG  MCLRE = ON            ; MCLR Pin Enable bit (MCLR pin enabled; RE3 input pin disabled)    

    ; CONFIG4L
      CONFIG  STVREN = OFF          ; Stack Full/Underflow Reset Enable bit (Stack full/underflow will not cause Reset)
      CONFIG  LVP = OFF             ; Single-Supply ICSP Enable bit (Single-Supply ICSP disabled)
      CONFIG  ICPRT = OFF           ; Dedicated In-Circuit Debug/Programming Port (ICPORT) Enable bit (ICPORT disabled)
      CONFIG  XINST = OFF           ; Extended Instruction Set Enable bit (Instruction set extension and Indexed Addressing mode disabled (Legacy mode))    

    ; CONFIG5L
      CONFIG  CP0 = OFF             ; Code Protection bit (Block 0 (000800-001FFFh) is not code-protected)
      CONFIG  CP1 = OFF             ; Code Protection bit (Block 1 (002000-003FFFh) is not code-protected)
      CONFIG  CP2 = OFF             ; Code Protection bit (Block 2 (004000-005FFFh) is not code-protected)
      CONFIG  CP3 = OFF             ; Code Protection bit (Block 3 (006000-007FFFh) is not code-protected)  

    ; CONFIG5H
      CONFIG  CPB = OFF             ; Boot Block Code Protection bit (Boot block (000000-0007FFh) is not code-protected)
      CONFIG  CPD = OFF             ; Data EEPROM Code Protection bit (Data EEPROM is not code-protected)   

    ; CONFIG6L
      CONFIG  WRT0 = OFF            ; Write Protection bit (Block 0 (000800-001FFFh) is not write-protected)
      CONFIG  WRT1 = OFF            ; Write Protection bit (Block 1 (002000-003FFFh) is not write-protected)
      CONFIG  WRT2 = OFF            ; Write Protection bit (Block 2 (004000-005FFFh) is not write-protected)
      CONFIG  WRT3 = OFF            ; Write Protection bit (Block 3 (006000-007FFFh) is not write-protected)    

    ; CONFIG6H
      CONFIG  WRTC = OFF            ; Configuration Register Write Protection bit (Configuration registers (300000-3000FFh) are not write-protected)
      CONFIG  WRTB = OFF            ; Boot Block Write Protection bit (Boot block (000000-0007FFh) is not write-protected)
      CONFIG  WRTD = OFF            ; Data EEPROM Write Protection bit (Data EEPROM is not write-protected) 

    ; CONFIG7L
      CONFIG  EBTR0 = OFF           ; Table Read Protection bit (Block 0 (000800-001FFFh) is not protected from table reads executed in other blocks)
      CONFIG  EBTR1 = OFF           ; Table Read Protection bit (Block 1 (002000-003FFFh) is not protected from table reads executed in other blocks)
      CONFIG  EBTR2 = OFF           ; Table Read Protection bit (Block 2 (004000-005FFFh) is not protected from table reads executed in other blocks)
      CONFIG  EBTR3 = OFF           ; Table Read Protection bit (Block 3 (006000-007FFFh) is not protected from table reads executed in other blocks)   

    ; CONFIG7H
      CONFIG  EBTRB = OFF           ; Boot Block Table Read Protection bit (Boot block (000000-0007FFh) is not protected from table reads executed in other blocks) 

      ;*****Variables Definition************
    DATA_A	EQU	0X000
    DATA_B	EQU	0x001
    RESULT	EQU	0x002
    REGISTER EQU	0X003
    DELAYN	EQU	0X004
    DELAYN2	EQU	0X005
    DELAYN3	EQU	0X006
    DELAYN4	EQU	0X007
    VELOCIDAD   EQU	0X008
    COUNTER	EQU	0X009
    LED_OSC	EQU	0XA
    CONSTANT    MASK    =b'00000011'
    CONSTANT    MASK2    =b'00000001'  
    ;*****Main code**********
    			ORG     0x000             	;reset vector
      			GOTO    MAIN              	;go to the main routine
    INITIALIZE:
    		MOVLW	0XF
    		MOVWF	ADCON1
    		MOVLW	0X000	    ;
    		MOVWF	DELAYN
    		SETF	TRISA	    ;THE WHOLE PORT A IS AN INPUT
    		CLRF	TRISB	    ;THE WHOLE PORT B IS AN OUTPUT
    		CLRF	TRISD	    ;THE WHOLE PORT D IS AN OUTPUT
    		MOVLW	0X009
    		MOVWF	COUNTER
    		MOVLW	0X001
    		MOVWF	VELOCIDAD
    		MOVLW	b'00000001'
    		MOVWF	PORTB
    		RETURN
    MAIN:           
    		CALL INITIALIZE 
    
    LOOP:				
    		MOVLW	0X00
    		CPFSGT	COUNTER
    		GOTO	CONTADOR9
    		MOVLW	0X01
    		CPFSLT	COUNTER
    		DECF	COUNTER
    F:
    		CALL	DECODE
    		CALL	LEDVEL
    		MOVF	PORTA, W	
    		ANDLW	MASK		
    		MOVWF	DATA_A
    		BZ  CHECKVEL
    		BTFSC	PORTA,0
    		GOTO	VERIF_A
    		BTFSC	PORTA,1
    		GOTO	VERIF_B
    		GOTO LOOP
    CONTADOR9:
    		MOVLW	0X09
    		MOVWF	COUNTER
    		GOTO	F
    VERIF_A:
    		BTFSC	PORTA, 0	;this verifies if another button was pressed 
    		GOTO	VERIF_A	    ;aqui mandar a subir la velocidad con método extra	
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
    		BTFSC	PORTA, 1	;this verifies if another button was pressed 
    		GOTO	VERIF_B	    ;aqui mandar a subir la velocidad con método extra	
    		MOVLW	0X1
    		CPFSLT	VELOCIDAD
    		DECF	VELOCIDAD, 1	;CHECAR NO REBASAR EL 5
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
    CHECKVEL:
    		CALL	DELAY100MS	;0.1 S
    		MOVLW	0X01
    		CPFSGT	VELOCIDAD
    		GOTO	LOOP
    		CALL	DELAY100MS	;0.5S
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		MOVLW	0X02
    		CPFSGT	VELOCIDAD
    		GOTO	LOOP
    		CALL	DELAY100MS	;1 S
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		MOVLW	0X03
    		CPFSGT	VELOCIDAD
    		GOTO	LOOP
    		CALL	DELAY1S		;5 S
    		CALL	DELAY1S
    		CALL	DELAY1S
    		CALL	DELAY1S
    		MOVLW	0X04
    		CPFSGT	VELOCIDAD
    		GOTO	LOOP
    		CALL	DELAY1S		;10 S
    		CALL	DELAY1S
    		CALL	DELAY1S
    		CALL	DELAY1S
    		CALL	DELAY1S
    		GOTO	LOOP
    DELAY1S:
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		CALL	DELAY100MS
    		RETURN
    DELAY100MS:
    		NOP
    		MOVLW	0X0A
    		MOVWF	DELAYN3
    		CALL DELAY10MS
    		RETURN
    DELAY10MS:
    		NOP
    		MOVLW	0X63
    		MOVWF	DELAYN2
    		CALL	DELAY100US
    		DECFSZ	DELAYN3
    		GOTO DELAY10MS
    		RETURN
    DELAY100US:			;OK
    		NOP
    		MOVLW	0X63
    		MOVWF	DELAYN
    		CALL DELAY1US
    		DECFSZ	DELAYN2
    		GOTO DELAY100US
    		RETURN
    DELAY1US:
    		NOP
    		NOP
    		NOP
    		DECFSZ	DELAYN
    		GOTO DELAY1US
    		RETURN
    
    DECODE: 
    		MOVLW	0X00
    		CPFSGT	COUNTER
    		GOTO	CERO
    		MOVLW	0X01
    		CPFSGT	COUNTER
    		GOTO	UNO
    		MOVLW	0X02
    		CPFSGT	COUNTER
    		GOTO	DOS
    		MOVLW	0X03
    		CPFSGT	COUNTER
    		GOTO	TRES
    		MOVLW	0X04
    		CPFSGT	COUNTER
    		GOTO	CUATRO
    		MOVLW	0X05
    		CPFSGT	COUNTER
    		GOTO	CINCO
    		MOVLW	0X06
    		CPFSGT	COUNTER
    		GOTO	SEIS
    		MOVLW	0X07
    		CPFSGT	COUNTER
    		GOTO	SIETE
    		MOVLW	0X08
    		CPFSGT	COUNTER
    		GOTO	OCHO
    		MOVLW	0X09
    		CPFSGT	COUNTER
    		GOTO	NUEVE
    CERO: 
    			 ;hgfedcba
    		MOVLW	b'00111111'
    		MOVWF	PORTD
    		RETURN  

    UNO: 
    		MOVLW	b'00000110'
    		MOVWF	PORTD
    		RETURN 
    
    DOS: 
    		MOVLW	b'01011011'
    		MOVWF	PORTD
    		RETURN
    
    TRES: 
    		MOVLW	b'01001111'
    		MOVWF	PORTD
    		RETURN  

    CUATRO: 
    		MOVLW	b'01100110'
    		MOVWF	PORTD
    		RETURN  

    CINCO: 
    		MOVLW	b'01101101'
    		MOVWF	PORTD
    		RETURN
    
    SEIS: 
    		MOVLW	b'01111101'
    		MOVWF	PORTD
    		RETURN
    
    SIETE: 
    		MOVLW	b'00000111'
    		MOVWF	PORTD
    		RETURN
    
    OCHO: 
    		MOVLW	b'01111111'
    		MOVWF	PORTD
    		RETURN
    
    NUEVE: 
    		MOVLW	b'01101111'
    		MOVWF	PORTD
    		RETURN
    LEDVEL:
    		INCF	LED_OSC, 1
    		;MOVF	LED_OSC, W
    		;ANDLW	MASK2
    		BTFSC	LED_OSC, 0
    		GOTO	LED_ON
    		GOTO	LED_OFF
    LED_ON:
    		BSF	PORTB, 5
    		RETURN
    LED_OFF:
    		BCF	PORTB, 5
    		RETURN		
    LED1:
    		MOVLW	b'00000001'
    		MOVWF	PORTB
    		GOTO CHECKVEL
    LED2:
    		MOVLW	b'00000010'
    		MOVWF	PORTB
    		GOTO CHECKVEL
    LED3:
    		MOVLW	b'00000100'
    		MOVWF	PORTB
    		GOTO CHECKVEL
    LED4:
    		MOVLW	b'00001000'
    		MOVWF	PORTB
    		GOTO CHECKVEL
    LED5:
    		MOVLW	b'00010000'
    		MOVWF	PORTB
    		GOTO CHECKVEL
    		END

# Simulación
Link: [Video de Youtube](https://youtu.be/20Z0WnfhyOk)

# Implementación
![Implementación](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica2/Implementacion1.jpeg)
![Osciloscopio](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1ctica2/Implementacion2.jpeg)
Link: [Video de Youtube](https://youtu.be/lYw_6ewRUtg)
