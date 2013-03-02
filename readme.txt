Paul started hacking this 28/2/2013

The plan:
	- build fir as a library, link it in and call funcs therein
	-introduce another ISR, doing the processing
	-using register_handler_ex(interrupt ik_ivg10, processing_isr, EX_INT_ENABLE) register it
	as lower priority (e.g. 10) than the SPORT ISR
	-when a buffer is full use raise(SIGIVG15), for example, to invoke it
	-make sure it is not self-reentrant, so cannot start processing another buffer if previous
	not cooked
	
Current dependencies list:
..\..\..\..\Blackfin/ldf/adsp-BF533.ldf
..\..\..\include\builtins.h
..\..\..\include\ccblkfn.h
..\..\..\include\cdef_LPBlackfin.h
..\..\..\include\cdefBF532.h
..\..\..\include\cdefBF533.h
..\..\..\include\def_LPBlackfin.h
..\..\..\include\defBF532.h
..\..\..\include\fr2x16_typedef.h
..\..\..\include\fract_typedef.h
..\..\..\include\r2x16_typedef.h
..\..\..\include\raw_typedef.h
..\..\..\include\stdlib.h
..\..\..\include\stdlib_bf.h
..\..\..\include\sys\anomaly_macros_rtl.h
..\..\..\include\sys\builtins_support.h
..\..\..\include\sys\exception.h
..\..\..\include\sys\mc_typedef.h
..\..\..\include\sysreg.h
..\..\..\include\yvals.h
..\..\..\lib\bf532_rev_0.5\__initsbsz532.doj
..\..\..\lib\bf532_rev_0.5\crtn532y.doj
..\..\..\lib\bf532_rev_0.5\crtsf532y.doj
..\..\..\lib\bf532_rev_0.5\Debug\libdrv532y.dlb
..\..\..\lib\bf532_rev_0.5\Debug\libssl532y.dlb
..\..\..\lib\bf532_rev_0.5\Debug\libusb532y.dlb
..\..\..\lib\bf532_rev_0.5\libc532y.dlb
..\..\..\lib\bf532_rev_0.5\libcpp532y.dlb
..\..\..\lib\bf532_rev_0.5\libdsp532y.dlb
..\..\..\lib\bf532_rev_0.5\libetsi532y.dlb
..\..\..\lib\bf532_rev_0.5\libevent532y.dlb
..\..\..\lib\bf532_rev_0.5\libf64ieee532y.dlb
..\..\..\lib\bf532_rev_0.5\libio532y.dlb
..\..\..\lib\bf532_rev_0.5\libprofile532y.dlb
..\..\..\lib\bf532_rev_0.5\librt_fileio532y.dlb
..\..\..\lib\bf532_rev_0.5\libsftflt532y.dlb
..\..\..\lib\bf532_rev_0.5\libsmall532y.dlb
..\..\..\lib\cplbtab533.doj
Initialize.c
ISR.c
main.c
Process_data.c
Talkthrough.h

	
	
_______________________________________________________________________________

ADSP-BF533 EZ-KIT Lite C Based AD1836 TALKTHRU

Analog Devices, Inc.
DSP Division
Three Technology Way
P.O. Box 9106
Norwood, MA 02062

Date Created:	04/03/03

_______________________________________________________________________________

This directory contains an example ADSP-BF533 subroutine that demonstrates the 
initialization of the link between SPORT0 on the ADSP-BF533 and the AD1836 
stereo Codec.  This is done by initializing the link in TDM mode and implementing
a simple talk-through routine.  

Files contained in this directory:

readme.txt				this file
main.c					C file containing the main program and variable declaration
Initialize.c			C file containing all initialization routines
ISR.c					C file containing the interrupt service routine for 
						SPORT0_RX
Process_data.c			C file for processing incoming data
Talkthrough.h			C header file containing prototypes and macros
C_Talkthrough_TDM.dpj	VisualDSP++ project file
_______________________________________________________________________________


CONTENTS

I.		FUNCTIONAL DESCRIPTION
II.		IMPLEMENTATION DESCRIPTION
III.	OPERATION DESCRIPTION


I.    FUNCTIONAL DESCRIPTION

The Talkthru demo demonstrates the initialization of SPORT0 to establish a link 
between the ADSP-BF533 and the AD1836 Codec.
 
The program simpley sets up the SPI port to configure the AD1836 codec to TDM mode.
SPORT0 is then setup to receive/transmit audio samples from the AD1836.  Audio
samples received from the AD1836 are moved into the DSP's receive buffer, using 
DMA.  The samples are processed by the ADSP-BF533 and place in the transmit buffer.
In turn, the transmit buffer is used to transmit data to the AD1836.  This results
in a simple talk-through where data is moved in and out of the DSP with out 
performing any computations on the data.


II.   IMPLEMENTATION DESCRIPTION

The Initialization module initializes:

1. EBIU setup 
2. Flash setup
3. SPI setup to initialize AD1836 codec
4. SPORT0 setup
5. DMA setup
6. Interrupt configuration


III.  OPERATION DESCRIPTION

- Open the project "C_Talkthrough_TDM.dpj" in the VisualDSP Integrated Development
	Environment (IDDE).
- Under the "Project" tab, select "Build Project" (program is then loaded 
	automatically into DSP).
- Turn pin 5 and 6 of SW9 on the ADSP-BF533 EZ-KIT Lite OFF; these will disconnect 
	RSCLK0 -> TSCLK0 and RFS0 -> TFS0.  	
- Setup an input source (such as a radio) to the audio in jack and an output 
	source (such as headphones) to the audio out jack of the EZ-KIT Lite.  See the
	ADSP-BF533 EZ-KIT Lite User's Manual for more information on setting up the
	hardware.
- Select "Run" from the "Debug" tab on the menu bar of VisualDSP.
- Listen to the operation of the talk-through
