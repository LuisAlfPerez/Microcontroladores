# Microcontroladores
# Práctica Integradora 2

El objetivo de esta práctica es implementar un sistema basado en microcontrolador. Dicho sistema decrementará un contador cada segundo. Dicho tiempo podrá ser modificado por el usuario mediante el uso de 1 push-button, empezando en 3 minutos y reduciendo de a 1 segundo por cada presionada. El tiempo restante será mostrado en un arreglo de 3 displays de 7 segmentos. Además, se incluyeron 3 botones adiciones, uno para inicio, otro de pausa y un úlitmo de reset. 


## Explicación del Código.

# Esquemático
![Esquemático](https://github.com/LuisAlfPerez/Microcontroladores/blob/Pr%C3%A1cticaIntegradora1/esquematico.png)

# Conclusión
En conclusión, se logró el objetivo de la práctica haciendo uso de instrucciones como nop y teniendo en cuenta el tiempo de duración de cada instrucción con el reloj usado y su configuración en los configuration bits, tal como en la [Práctica 4](https://github.com/LuisAlfPerez/Microcontroladores/tree/Pr%C3%A1ctica4). Sin embargo, ahora se usó esa lógica para realizar un timer capaz de contar desde los 3 minutos, que se puede pausar, reiniciar, reanudar y personalizar el tiempo.


## Código
	#include "mcc.h"
	#include "stdio.h"
	#include "stdlib.h"
	#include <xlcd.h>

	void c4_do(){
	    switch (velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(359);
		    PORTCbits.RC1 = 0;
		    __delay_us(359);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(478);
		    PORTCbits.RC1 = 0;
		    __delay_us(478);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(598);
		    PORTCbits.RC1 = 0;
		    __delay_us(598);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default:
		break;
	    }
	}


	void d4_re(){
	    switch (velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(320);
		    PORTCbits.RC1 = 0;
		    __delay_us(320);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(426);
		    PORTCbits.RC1 = 0;
		    __delay_us(426);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(533);
		    PORTCbits.RC1 = 0;
		    __delay_us(533);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default:
		    break;
	    }
	}

	void e4_mi(){
	    switch(velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(285);
		    PORTCbits.RC1 = 0;
		    __delay_us(285);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(380);
		    PORTCbits.RC1 = 0;
		    __delay_us(380);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(475);
		    PORTCbits.RC1 = 0;
		    __delay_us(475);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default:
		    break;
	    }
	}

	void f4_fa(){
	    switch (velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(268);
		    PORTCbits.RC1 = 0;
		    __delay_us(268);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(358);
		    PORTCbits.RC1 = 0;
		    __delay_us(358);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(447);
		    PORTCbits.RC1 = 0;
		    __delay_us(447);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default: 
		    break;
	    }
	}

	void g4_sol(){
	    switch (velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(234);
		    PORTCbits.RC1 = 0;
		    __delay_us(234);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(312);
		    PORTCbits.RC1 = 0;
		    __delay_us(312);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(394);
		    PORTCbits.RC1 = 0;
		    __delay_us(394);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default: 
		    break;
	    }
	}

	void a4_la(){
	    switch (velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(213);
		    PORTCbits.RC1 = 0;
		    __delay_us(213);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(284);
		    PORTCbits.RC1 = 0;
		    __delay_us(284);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(355);
		    PORTCbits.RC1 = 0;
		    __delay_us(355);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default: 
		    break;
	    }
	}

	void b4_si(){
	    switch (velocidad){
		case 1:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(190);
		    PORTCbits.RC1 = 0;
		    __delay_us(190);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 2:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(253);
		    PORTCbits.RC1 = 0;
		    __delay_us(253);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		case 3:
		TMR0_StartTimer();
		while(TMR0Flag == 0){
		    PORTCbits.RC1 = 1;
		    __delay_us(316);
		    PORTCbits.RC1 = 0;
		    __delay_us(316);
		}
		TMR0Flag = 0;
		TMR0_StopTimer();
		break;
		default: 
		    break;
	    }
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
	    velocidad = 2;
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
Link: [Video de Youtube](https://youtu.be/-EAn2wpIkuU)
