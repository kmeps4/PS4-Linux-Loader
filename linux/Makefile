
all: payload-1000.bin payload-1000-2gb.bin payload-1000-3gb.bin payload-1000-4gb.bin

clean:
	rm -f payload-1000.elf payload-1000.bin payload-1000-2gb.elf payload-1000-2gb.bin payload-1000-3gb.elf payload-1000-3gb.bin payload-1000-4gb.elf payload-1000-4gb.bin; 
	cd ps4-kexec; make clean
	cd ../lib/; make clean
    
../lib/lib.a:
	cd ../lib; make


ps4-kexec/kexec.bin:
	cd ps4-kexec; make

payload-1000.elf: ../lib/lib.a main.c ps4-kexec/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__10_00__ -DPS4_10_00 main.c -Wl,-gc-sections -o payload-1000.elf -fPIE -ffreestanding

payload-1000-2gb.elf: ../lib/lib.a main.c ps4-kexec/kexec.bin
	 gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__10_00__ -DPS4_10_00 -DVRAM_GB_DEFAULT=2 main.c -Wl,-gc-sections -o payload-1000-2gb.elf -fPIE -ffreestanding

payload-1000-3gb.elf: ../lib/lib.a main.c ps4-kexec/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__10_00__ -DPS4_10_00 -DVRAM_GB_DEFAULT=3 main.c -Wl,-gc-sections -o payload-1000-3gb.elf -fPIE -ffreestanding

payload-1000-4gb.elf: ../lib/lib.a main.c ps4-kexec/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__10_00__ -DPS4_10_00 -DVRAM_GB_DEFAULT=4 main.c -Wl,-gc-sections -o payload-1000-4gb.elf -fPIE -ffreestanding


%.bin: %.elf
	objcopy $< --only-section .text --only-section .data --only-section .bss --only-section .rodata -O binary $@
	file $@ | fgrep -q '$@: DOS executable (COM)'

ps4-kexec-%/kexec.bin:
	cd ps4-kexec-$*; make
