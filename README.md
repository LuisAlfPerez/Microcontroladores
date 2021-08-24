# Microcontroladores
# Práctica 1

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
  	CONSTANT    MASK    =b'11110000'
  	CONSTANT    MASK2   =b'00001111'
    
  	;*****Main code**********
  				ORG     0x000             	;reset vector
  	  			GOTO    MAIN              	;go to the main routine
  	INITIALIZE:
  			MOVLW	0XF
  			MOVWF	ADCON1
  			SETF	TRISB	    ;THE WHOLE PORT B IS AN INPUT
  			CLRF	TRISD	    ;THE WHOLE PORT D IS AN OUTPUT		
		
  			RETURN
  	MAIN:           
  			CALL INITIALIZE 
		
  	LOOP:				
  			MOVF	PORTB, W	
  			ANDLW	MASK2		
  			MOVWF	DATA_A
		
  			MOVF	PORTB, W	
  			ANDLW	MASK
  			MOVWF	DATA_B
  			SWAPF	DATA_B, W
		
  			ADDWF	DATA_A, W
  			MOVWF	RESULT
		
  			CALL DECODE
		
  			GOTO LOOP

  	DECODE: 
  			MOVWF	RESULT
  			BZ	CERO
  			DECF	RESULT, W
  			BZ	UNO
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	DOS
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	TRES
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	CUATRO
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	CINCO
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	SEIS
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	SIETE
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	OCHO
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	NUEVE
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	DIEZ
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	ONCE
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	DOCE
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	TRECE
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	CATORCE
  			MOVWF	RESULT
  			DECF	RESULT, W
  			BZ	QUINCE
  			MOVWF	RESULT
  			GOTO	MAYOR
  	CERO: 
  				 ;hgfedcba
  			MOVLW	b'00111111'
  			MOVWF	PORTD
  			GOTO	LOOP

  	UNO: 
  			MOVLW	b'00000110'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	DOS: 
  			MOVLW	b'01011011'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	TRES: 
  			MOVLW	b'01001111'
  			MOVWF	PORTD
  			GOTO	LOOP

  	CUATRO: 
  			MOVLW	b'01100110'
  			MOVWF	PORTD
  			GOTO	LOOP

  	CINCO: 
  			MOVLW	b'01101101'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	SEIS: 
  			MOVLW	b'01111101'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	SIETE: 
  			MOVLW	b'00000111'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	OCHO: 
  			MOVLW	b'01111111'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	NUEVE: 
  			MOVLW	b'01101111'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	DIEZ: 
  			MOVLW	b'01110111'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	ONCE: 
  			MOVLW	b'01111100'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	DOCE: 
  			MOVLW	b'00111001'
  			MOVWF	PORTD
  			GOTO	LOOP

  	TRECE: 
  			MOVLW	b'01011110'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	CATORCE: 
  			MOVLW	b'01111001'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	QUINCE: 
  			MOVLW	b'01110001'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  	MAYOR: 
  			MOVLW	b'00100011'
  			MOVWF	PORTD
  			GOTO	LOOP
		
  			END
			
# Simulación Proteus
Link: [Video de Youtube](https://youtu.be/j6OHJU-kuHc)

# Video
Link: [Video de Youtube](https://www.youtube.com/embed/fEcTEmr6Qyo)

