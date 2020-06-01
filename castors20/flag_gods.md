**BANE ART** 


**Category:** Coding
**Concepts:** python, hamming distance

**Problem Statement:**
The flag gods are trying to tell you something...

nc chals20.cybercastors.com 14431

**Hints**
None

Netcat greeting:
```
 ______ __      ______  ______       ______  ______  _____   ______    
/\  ___/\ \    /\  __ \/\  ___\     /\  ___\/\  __ \/\  __-./\  ___\   
\ \  __\ \ \___\ \  __ \ \ \__ \    \ \ \__ \ \ \/\ \ \ \/\ \ \___  \  
 \ \_\  \ \_____\ \_\ \_\ \_____\    \ \_____\ \_____\ \____-\/\_____\ 
  \/_/   \/_____/\/_/\/_/\/_____/     \/_____/\/_____/\/____/ \/_____/ 
                                                                                                                                                             

We have a small problem...
The flag gods are trying to send us a message, but our transmitter isn't calibrated to
decode it! If you can find the hamming distance for the following messages we may be
able to calibrate the transmitter in time. Entering the wrong distance will lock the
machine. Good luck, we'll only have 20 seconds!
Hit <enter> when ready.

The machine is currently 20% calibrated.
Transmitted message: The clueless monkey hits a dragon occasionally.
Received message: 32f71afeccc30cf89598089d5d96b811904ac2ceb2979b88db9add9c0f8bd8c091d7d99d9a9c8f0692819ac35786c1
Enter hamming distance:
```

Hamming distance: Number of indices where 2 strings of equal length differ

So here, the strings are of different length. After a lot of hit and trials, I realized the hamming distance to measure was of the binary values of both the strings(hex->binary for the recieved message and ascii->binary for the transmitted message).

Now I just had to write a python code to automate this process(impossible manually because it is required to do this 80 times in 20 seconds).

So, I wrote this script

```python
from pwn import *
import base64
from scipy.spatial import distance

def hamming_distance(chaine1, chaine2):
	return str(sum(c1 != c2 for c1, c2 in zip(chaine1, chaine2)))

def hextobin(hexi):
	print(hexi)
	stri = ""
	# stri = bytes.fromhex(hexi).decode("ASCII")
	# return stri
	newl = []
	i = 0
	index = 0
	while i < len(hexi):
		# print(i)
		newl.append(hexi[i] + hexi[i+1])
		i+=2
		# index+=1

	for num in newl:
		stri+=str(f'{int(num,16):08b}') + " "
		# f'{160:08b}'
	return stri[:-1]

def strtobin(inp):
	stri = ""
	for i in inp:
		stri += str(f'{int(ord(i)):08b}') + " "

	return stri[:-1]


host = "chals20.cybercastors.com"
port = "14431"
count = 0
r = remote(host, port)

prompt = r.recvuntil("ready.")
r.send("\n")
count=1
print(prompt)

prompt = r.recvuntil("\n")
prompt = r.recvuntil("\n")
prompt = r.recvuntil("\n")
trans = str(prompt)[23:-3]
print("tiran ",trans)
prompt = r.recvuntil("\n")
recv = str(prompt)[20:-3]
print("reii ",recv)

bin1 = hextobin(recv)
bin2 = strtobin(trans)
print(bin1,"\n\n")
print(f"this{bin2}")
hd = hamming_distance(bin1,bin2)
print(hd)

r.sendline(hd)
prompt = r.recvuntil("\n")
prompt = r.recvuntil("\n")
print(prompt)

for i in range(79):
	prompt = r.recvuntil("\n")

	trans = str(prompt)[23:-3]
	print("tiran ",trans)
	prompt = r.recvuntil("\n")
	# prompt = r.recvuntil("\n")
	print(prompt)
	recv = str(prompt)[20:-3]
	print("reii ",recv)

	bin1 = hextobin(recv)
	bin2 = strtobin(trans)
	print(bin1,"\n\n")
	print(f"this{bin2}")
	hd = hamming_distance(bin1,bin2)
	print(hd,i)

	r.sendline(hd)
	if i == 78:
		r.interactive()
	prompt = r.recvuntil("\n")
	prompt = r.recvuntil("\n")
	print(prompt)

r.interactive()
```

**FLAG:** castorsCTF{c0mmun1ng_w17h_7h3_f14g_g0d5}


