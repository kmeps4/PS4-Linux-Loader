
all: payload-1100.bin payload-1100-2gb.bin payload-1100-3gb.bin payload-1100-4gb.bin

clean:
	rm -f payload-1100.elf payload-1100.bin payload-1100-2gb.elf payload-1100-2gb.bin payload-1100-3gb.elf payload-1100-3gb.bin payload-1100-4gb.elf payload-1100-4gb.bin; 
	cd ps4-kexec-1100; make clean
	cd ../lib/; make clean
    
../lib/lib.a:
	cd ../lib; make


ps4-kexec-1100/kexec.bin:
	cd ps4-kexec-1100; make

payload-1100.elf: ../lib/lib.a main.c ps4-kexec-1100/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__11_00__ -DPS4_11_00 main.c -Wl,-gc-sections -o payload-1100.elf -fPIE -ffreestanding

payload-1100-2gb.elf: ../lib/lib.a main.c ps4-kexec-1100/kexec.bin
	 gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__11_00__ -DPS4_11_00 -DVRAM_GB_DEFAULT=2 main.c -Wl,-gc-sections -o payload-1100-2gb.elf -fPIE -ffreestanding

payload-1100-3gb.elf: ../lib/lib.a main.c ps4-kexec-1100/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__11_00__ -DPS4_11_00 -DVRAM_GB_DEFAULT=3 main.c -Wl,-gc-sections -o payload-1100-3gb.elf -fPIE -ffreestanding

payload-1100-4gb.elf: ../lib/lib.a main.c ps4-kexec-1100/kexec.bin
	gcc -isystem ../freebsd-headers -nostdinc -nostdlib -fno-stack-protector -static ../lib/lib.a -D__11_00__ -DPS4_11_00 -DVRAM_GB_DEFAULT=4 main.c -Wl,-gc-sections -o payload-1100-4gb.elf -fPIE -ffreestanding


%.bin: %.elf
	objcopy $< --only-section .text --only-section .data --only-section .bss --only-section .rodata -O binary $@
	file $@ | fgrep -q '$@: DOS executable (COM)'

ps4-kexec-%/kexec.bin:
	cd ps4-kexec-$*; make
