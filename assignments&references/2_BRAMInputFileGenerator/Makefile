#Copyright (c) 2019 HaojunXia

#This script can change .S Files into .inst & .data files which can be loaded by our FPGA's Block memories.
#Decided upon which level of files you want, you can choose one of these commands listed below
	#make/make all: 
		#change all .S files into .data & .inst files, meanwhile get assembly code
	#make DataAndInst: 
		#dump both Data and Instructions and save them to .data files
	#make InstOnly: 
		#dump ONLY Instructions and save them to .inst files
	#make OMFile: 
		#assembly the .S files into .o files, then link the .o files into .om files
	#make OFile: 
		#assembly the .S files into .o files
	#make Objdump 
		#get the disassembly code of riscv test
#Other options:
	#make clean:
		#clean all files generated by this makefile, but do not delete .S files
	#make cleanASM:
		#clean all .S files

#Deal with .S Files
ASM_SOURCE = $(wildcard *.S)
INSTONLY = $(patsubst %.S,%.inst,$(ASM_SOURCE))
DATAANDINST = $(patsubst %.S,%.data,$(ASM_SOURCE))
OMFILE = $(patsubst %.S,%.om,$(ASM_SOURCE))
OFILE = $(patsubst %.S,%.o,$(ASM_SOURCE))
OBJDUMP = $(patsubst %.S,%.txt,$(ASM_SOURCE))
all: InstOnly DataAndInst Objdump cleanTMP
InstOnly: $(INSTONLY)
DataAndInst: $(DATAANDINST)
OMFile: $(OMFILE)
OFile: $(OFILE)
Objdump: $(OBJDUMP)

#rules
Bin2Data: ./Utils/Bin2Data.c
	gcc -o Bin2Data ./Utils/Bin2Data.c
%.o: %.S
	./Utils/riscv32-elf-as $< -o $@ -march=rv32i
%.om: %.o
	./Utils/riscv32-elf-ld $< -o $@
%-Data.bin: %.om
	./Utils/riscv32-elf-objcopy -O binary $< $@
%-Inst.bin: %.om
	./Utils/riscv32-elf-objcopy --dump-section .text=$@ $<
%.data: %-Data.bin Bin2Data
	./Bin2Data $< $@
%.inst: %-Inst.bin Bin2Data
	./Bin2Data $< $@
%.txt: %.om
	./Utils/riscv32-elf-objdump -d $< > $@
cleanTMP:
	-rm -f *.om *.o *.bin Bin2Data
clean:
	-rm -f *.data *.inst *.txt *.om *.o *.bin Bin2Data
cleanASM:
	-rm -f *.S

.SUFFIXES:

