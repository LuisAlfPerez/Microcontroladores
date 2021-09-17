# Microcontroladores
# Práctica Integradora 1

El objetivo de esta práctica es implementar un sistema basado en microcontrolador. Dicho sistema decrementará un contador cada segundo. Dicho tiempo podrá ser modificado por el usuario mediante el uso de 1 push-button, empezando en 3 minutos y reduciendo de a 1 segundo por cada presionada. El tiempo restante será mostrado en un arreglo de 3 displays de 7 segmentos. Además, se incluyeron 3 botones adiciones, uno para inicio, otro de pausa y un úlitmo de reset. 


## Explicación del Código.

Para lograr la práctica se debieron de considerar los siguientes pasos:
1. Cálculo de tiempo de cada instrucción.
2. Declaración de puertos E/S
3. Decrementar la cuenta cada segundo.
4. Realizar la salida binaria para que, después del convertidor, se muestre en los displays de 7seg.
5. Verificar el estado de los botones, para cada acción pertinente.

### Paso 1: Cálculo de tiempo de cada instrucción.
Tomando en cuenta un cristal de 20MHz, se hace una conversión en los bits de configuracion resultando en un reloj de 24MHz. Puesto que cada instrucción ocupa 4 ciclos, se obtienen 6M de instrucciones. Sacando el inverso, se obtiene que el tiempo por instrucción es de 166ns.

### Paso 2: Declaración de puertos E/S.
Para ello, el primer paso de todo el programa, es definir nuestros puertos de E/S

	SETF	TRISA	    ;THE WHOLE PORT A IS AN INPUT
	CLRF	TRISB	    ;THE WHOLE PORT B IS AN OUTPUT UNIDADES
	CLRF	TRISC	    ;THE WHOLE PORT C IS AN OUTPUT MIN
	CLRF	TRISD	    ;THE WHOLE PORT D IS AN OUTPUT MIN

### Paso 3: Inicializar el tiempo en 3 minutos.
En el loop, se verifica el estado del contador. En caso de ser igual a 0, entra a una función que resetea la cuenta a 0. De lo contrario, se decrementará y seguirá al siguiente paso.

	MOVLW	0X03
	MOVWF	PORTC	    ;SET TO 3 MINUTES
	
### Paso 4: Verificar si está presionado el botón de pausa.

	VERIF_PAUSE:
		BTFSS	PORTA, 0	
		RETURN
		GOTO	PAUSE

En caso de estar presionado, se espera hasta que el botón haya sido soltado:

	PAUSE:
		BTFSC	PORTA, 0	
		GOTO	PAUSE
		GOTO	MENU_PAUSED
		
Y en cuanto se libere, se para a un menú de pausa, esperando a que haya un nuevo click del usuario:

	MENU_PAUSED:
		BTFSC	PORTA, 1
		GOTO	VERIF_PLAY
		BTFSC	PORTA, 2 
		GOTO	VERIF_RESET
		BTFSC	PORTA, 3
		GOTO	VERIF_LOAD
		GOTO	MENU_PAUSED
		
### Paso 5: Realizar la acción solicitada por el usuario.
El primer paso es verificar si ya se ha liberado el botón, sin importar cuál haya sido presionado. Para el caso de pausa lo único que hace es regresarlo al loop principal, dejándolo así fuera del menú de pausa.

	VERIF_PLAY:
		BTFSC	PORTA, 1	
		GOTO	VERIF_PLAY
		GOTO	LOOP
		
Para el reset, lo que se hace es regresar el valor de 3 minutos y se regresa al menú de pausa a espera del play, o a que se hagan cambios al tiempo.
		
	VERIF_RESET:	
		BTFSC	PORTA, 2 
		GOTO	VERIF_RESET
		CLRF	PORTB	    
		CLRF	PORTD	    
		MOVLW	0X03
		MOVWF	PORTC	    ;SET TO 3 MINUTES
		GOTO	MENU_PAUSED
		
En el caso de load, se decrementa llamando a la función COMPARE_UNI (que se muestra a continuación), aunque sigue en pausa. 
		
	VERIF_LOAD:
		BTFSC	PORTA, 3	
		GOTO	VERIF_LOAD
		CALL	COMPARE_UNI
		GOTO	MENU_PAUSED
		
### Paso 6: Reducir una unidad al tiempo y asignar el valor correcto de minutos y segundos.
Se reduce una unidad al digito menos significativo del segundo, en caso de ser mayor a 0, o se cambia para reducir las decenas.

	COMPARE_UNI:
		MOVLW	0X00
		CPFSGT	PORTB
		GOTO	COMPARE_DEC
		DECF	PORTB
		CLRF	PORTA
		RETURN

Se repite el mismo procedimiento, pero ahora la comparación es entre las decenas de segundo y el minuto.		
		
	COMPARE_DEC:
		MOVLW	0X00
		CPFSGT	PORTD
		GOTO	DEC_MIN
		MOVLW	0X09
		MOVWF	PORTB
		DECF	PORTD
		CLRF	PORTA
		RETURN
	
Se verifica que el tiempo no se haya agotado, si así fue se manda automáticamente al menú de pausa. Si aún hay minutos, solo se decrementa y sigue la cuenta regresiva. 
	
	DEC_MIN:
		MOVLW	0X00
		CPFSGT	PORTC	
		GOTO	MENU_PAUSED
		MOVLW	0x05
		MOVWF	PORTD
		MOVLW	0X09
		MOVWF	PORTB
		DECF	PORTC
		CLRF	PORTA
		RETURN


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
![Esquemático](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1cticaIntegradora1/esquematico.png)

# Conclusión
En conclusión, se logró el objetivo de la práctica haciendo uso de instrucciones como nop y teniendo en cuenta el tiempo de duración de cada instrucción con el reloj usado y su configuración en los configuration bits, tal como en la [Práctica 2](https://github.com/LuisAlfPerez/Microcontroladores/tree/Pr%C3%A1ctica2). Sin embargo, ahora se usó esa lógica para realizar un timer capaz de contar desde los 3 minutos, que se puede pausar, reiniciar, reanudar y personalizar el tiempo.


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
	DELAYN	EQU	0X000
	DELAYN2	EQU	0X001
	DELAYN3	EQU	0X002
	DELAYN4	EQU	0X003
	LED_OSC	EQU	0X004
	;*****Main code**********
				ORG     0x000             	;reset vector
				GOTO    MAIN              	;go to the main routine
	INITIALIZE:
			MOVLW	0XF
			MOVWF	ADCON1
			MOVLW	0X000	    ;
			MOVWF	DELAYN
			SETF	TRISA	    ;THE WHOLE PORT A IS AN INPUT
			CLRF	TRISB	    ;THE WHOLE PORT B IS AN OUTPUT UNIDADES
			CLRF	TRISC	    ;THE WHOLE PORT C IS AN OUTPUT MIN
			CLRF	TRISD	    ;THE WHOLE PORT D IS AN OUTPUT DEC
			CLRF	PORTB	    
			CLRF	PORTD	    
			MOVLW	0X03
			MOVWF	PORTC	    ;SET TO 3 MINUTES
			RETURN
	MAIN:           
			CALL	INITIALIZE 
			GOTO	PAUSE

	LOOP:			
			CALL	VERIF_PAUSE
			CALL	COMPARE_UNI
			CALL	DELAY1S
			GOTO	LOOP

	VERIF_PAUSE:
			BTFSS	PORTA, 0	
			RETURN
			GOTO	PAUSE

	PAUSE:
			BTFSC	PORTA, 0	
			GOTO	PAUSE
			GOTO	MENU_PAUSED

	MENU_PAUSED:
			BTFSC	PORTA, 1
			GOTO	VERIF_PLAY
			BTFSC	PORTA, 2 
			GOTO	VERIF_RESET
			BTFSC	PORTA, 3
			GOTO	VERIF_LOAD
			GOTO	MENU_PAUSED

	VERIF_PLAY:
			BTFSC	PORTA, 1	
			GOTO	VERIF_PLAY
			GOTO	LOOP

	VERIF_RESET:	
			BTFSC	PORTA, 2 
			GOTO	VERIF_RESET
			CLRF	PORTB	    
			CLRF	PORTD	    
			MOVLW	0X03
			MOVWF	PORTC	    ;SET TO 3 MINUTES
			GOTO	MENU_PAUSED

	VERIF_LOAD:
			BTFSC	PORTA, 3	
			GOTO	VERIF_LOAD
			CALL	COMPARE_UNI
			GOTO	MENU_PAUSED

	COMPARE_UNI:
			MOVLW	0X00
			CPFSGT	PORTB
			GOTO	COMPARE_DEC
			DECF	PORTB
			CLRF	PORTA
			RETURN



	COMPARE_DEC:
			MOVLW	0X00
			CPFSGT	PORTD
			GOTO	DEC_MIN
			MOVLW	0X09
			MOVWF	PORTB
			DECF	PORTD
			CLRF	PORTA
			RETURN

	DEC_MIN:
			MOVLW	0X00
			CPFSGT	PORTC	
			GOTO	MENU_PAUSED
			MOVLW	0x05
			MOVWF	PORTD
			MOVLW	0X09
			MOVWF	PORTB
			DECF	PORTC
			CLRF	PORTA
			RETURN

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

	end
# Simulación
Link: [Video de Youtube](https://youtu.be/oDA362vVDwo)

# Implementación
Link: [Video de Youtube](https://youtu.be/-EAn2wpIkuU)
