### Scan Surprise — picoCTF Writeup

picoctf problems :
https://play.picoctf.org/practice?page=2




### Description

I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_atlas/1/challenge.zip)

The same files are accessible via SSH here:

```
ssh -p 62406 ctf-player@atlas.picoctf.net
```

Using the password

```
83dcefb7
```

. Accept the fingerprint with

```
yes
```

, and

```
ls
```



## Steps I followed (easy words + commands)

1. **Open a terminal** on your computer.
2. **Connect to the remote challenge host via SSH** (you ran this):

```bash
ssh -p 60961 ctf-player@atlas.picoctf.net

```

1. **Accept the host key fingerprint**
    
    When SSH asked:
    

```
The authenticity of host 'atlas.picoctf.net (IP)' can't be established.
ECDSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?

```

You typed:

```
yes

```

(That adds the host to `~/.ssh/known_hosts`.)

1. **Enter the password when prompted**
    
    SSH then asked for the password. You typed the password (note: characters do **not** show while you type — this is normal):
    

```
Password: 83dcefb7

```

> Reminder: shells hide password characters when you type for security; it looks like nothing is being typed but it is.

1. **List files in the directory**
    
    After logging in you ran:
    

```bash
ls

```

Output:

```
flag.png

```

1. **Check the file type**
    
    You ran:
    

```bash
file flag.png

```

Output:

```
flag.png: PNG image data, 99 x 99, 1-bit colormap, non-interlaced

```

(So it’s a small black-and-white PNG — looks like a QR code.)

1. **Decode the QR code with a command-line QR reader**
    
    You ran:
    

```bash
zbarimg flag.png

```

`zbarimg` produced:

```
Connection Error (Failed to connect to socket /var/run/dbus/system_bus_socket: No such file or directory)
Connection Null
QR-Code:picoCTF{Flag}
scanned 1 barcode symbols from 1 images in 0.01 seconds

```

The important line is:

```
QR-Code:picoCTF{Flag}

```

— that is the **flag**.



⚠️ Safety note
--
I confirm that I have NOT published any sensitive credentials or harmful instructions. This writeup only shows how to safely inspect and decode an image to extract a flagged string for learning purposes. The flag itself has been redacted in this document.
