# beginner_reverse – Insomni’hack teaser 2019

There are lots of noisy codes in the context (later on, I learned that this is so-called Rust).

After examining the whole code, I found the section that is the key of this challenge– void beginer_reverse::main::h80fa15281f646bc1()

(Which is above the main() if you opened the program in IDAPro)

where it did the mathematic:

	do{
    if ( v16 == v24 )
      break;
    v2 = ((*(_DWORD *)(v33 + 4 * v25) >> 2) ^ 0xA) == *(_DWORD *)(v16 + 4 * v25);
    ++v25;
    v26 += v2;
    v24 -= 4LL;
    }
    
with the initializers:
    
    *(_OWORD *)v0 = xmmword_51000;
    *(_OWORD *)(v0 + 16) = xmmword_51010;
    *(_OWORD *)(v0 + 32) = xmmword_51020;
    *(_OWORD *)(v0 + 48) = xmmword_51030;
    *(_OWORD *)(v0 + 64) = xmmword_51040;
    *(_OWORD *)(v0 + 80) = xmmword_51050;
    *(_OWORD *)(v0 + 96) = xmmword_51060;
    *(_OWORD *)(v0 + 112) = xmmword_51070;
    *(_QWORD *)(v0 + 128) = 2052994367970LL;
   
 diving into those initializers:
 
    .rodata:0000000000051000 xmmword_51000   xmmword 1C600000166000001120000010Eh
    .rodata:0000000000051000                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+29r
    .rodata:0000000000051010 xmmword_51010   xmmword 1E2000001FE000000EA000001CEh
    .rodata:0000000000051010                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+33r
    .rodata:0000000000051020 xmmword_51020   xmmword 1E200000156000001AE00000156h
    .rodata:0000000000051020                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+3Er
    .rodata:0000000000051030 xmmword_51030   xmmword 156000000EE000001AE000000E6h
    .rodata:0000000000051030                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+49r
    .rodata:0000000000051040 xmmword_51040   xmmword 1BA000001E2000000FA0000018Ah
    .rodata:0000000000051040                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+54r
    .rodata:0000000000051050 xmmword_51050   xmmword 0E6000001E2000000EA000001A6h
    .rodata:0000000000051050                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+5Fr
    .rodata:0000000000051060 xmmword_51060   xmmword 1F2000000E6000001E200000156h
    .rodata:0000000000051060                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+6Ar
    .rodata:0000000000051070 xmmword_51070   xmmword 0E6000001E6000001E2000000E6h
    .rodata:0000000000051070                                         ; DATA XREF: beginer_reverse::main::h80fa15281f646bc1+75r
    
    
with the little endian:

flag = [0x10e, 0x112, 0x166, 0x1c6, 0x1ce, 0xea, 0x1fe, 0x1e2, 0x156, 0x1ae, 0x156, 0x1e2, 0xe6, 0x1ae, 0xee, 0x156, 0x18a, 0xfa, 0x1e2, 0x1ba, 0x1a6, 0xea, 0x1e2, 0xe6, 0x156, 0x1e2, 0xe6, 0x1f2, 0xe6, 0x1e2, 0x1e6, 0xe6]

so my code is:

    flag = [0x10e, 0x112, 0x166, 0x1c6, 0x1ce, 0xea, 0x1fe, 0x1e2, 0x156, 0x1ae, 0x156, 0x1e2, 0xe6, 0x1ae, 0xee, 0x156, 0x18a, 0xfa, 0x1e2, 0x1ba, 0x1a6, 0xea, 0x1e2, 0xe6, 0x156, 0x1e2, 0xe6, 0x1f2, 0xe6, 0x1e2, 0x1e6, 0xe6]
    final_flag=''    
    for i in flag:
    	final_flag += chr(( i >> 2) ^10)
    
    print (final_flag)
    INS{y0ur_a_r3a1_h4rdc0r3_r3v3rs3
    
Here, I’d need to admit that for the last two characters, I guess.

###INS{y0ur_a_r3a1_h4rdc0r3_r3v3rs3r}

Then I passed.