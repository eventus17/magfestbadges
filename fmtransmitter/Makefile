
all : test.hex burn test.lst
PART=attiny85
PROGPART=t85

CFLAGS=-g -Wall -pedantic -Os -mmcu=$(PART) -DF_CPU=8000000UL
CC=avr-gcc

test.elf : test.o
	avr-gcc -I  -g -Wall -pedantic -Os    -mmcu=$(PART) -Wl,-Map,test.map -o $@ $^ -L/usr/lib/binutils/avr/2.18

test.hex : test.elf
	avr-objcopy -j .text -j .data -O ihex test.elf test.hex 


test.lst : test.c
	avr-gcc -c -g -Wa,-a,-ad $(CFLAGS) $^ > $@

burn : test.hex
	avrdude -c usbtiny -p $(PROGPART) -U flash:w:test.hex

#For PLL
burnfuses :
	avrdude -c usbtiny -p $(PROGPART) -U lfuse:w:0xd1:m -U hfuse:w:0xdf:m -U efuse:w:0xff:m 

burnfuses_default :
	avrdude -c usbtiny -p $(PROGPART) -U lfuse:w:0x62:m -U hfuse:w:0xdf:m -U efuse:w:0xff:m 

readfuses :
	avrdude -c usbtiny -p $(PROGPART) -U hfuse:r:high.txt:b -U lfuse:r:low.txt:b

clean : 
	rm -f *~ high.txt low.txt test.hex test.map test.elf *.o
