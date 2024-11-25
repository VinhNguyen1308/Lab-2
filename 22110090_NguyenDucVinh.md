# Lab #2,22110090, Nguyen Duc Vinh, INSE330380E_01FIE
# Task 1: Transfer files between computers


**Question 1**: Conduct transfering a single plaintext file between 2 computers, 
Using openssl to implementing measures manually to ensure file integerity and authenticity at sending side, 
then veryfing at receiving side.

**Answer 1**:
## 1. Create a text file named `file.txt`:
### Preparing step
**Creating Network** <br>
`docker network create my_network`
<br>

**Create 2 containers for "Sender" and "Reciever"**   
  ```sh
docker run -it --name sender --network my_network ubuntu
docker run -it --name receiver --network my_network ubuntu
``` 
### Step 1: Create a text file named file.txt in container sender
<br>
Firstly, create a txt file with a content inside. Then Save text file as .txt

```sh
 echo "I have a apple pen" > file.txt
 ```
 Then the result display below

 <img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/811e0f07-495a-4e61-b9df-78540a77172d"><br>


### Step 2: Install `netcat-traditional` for "sender" and "receiver"
Using Bridge Network to transfer file from "Sender" to "Receiver"

```sh
apt-get update && apt-get install netcat-traditional -y
```
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/0f10c2d3-2a07-4505-a163-31b44bf90873 "><br>


### Step 3: Send file `file.txt` by using Netcat:

```sh
cat file.txt | nc receiver 12345

```

While "reciever" waiting to receive the file via Netcat:
```sh
nc -l -p 12345 > file.txt
```
After finishing transfering a single plaintext file between 2 computers, the result will display like below

**Sender**
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/25d2dee0-98d7-46f3-a2b4-019080c62690 "><br>


**Receiver**
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/c119f59d-a064-4040-a197-091d0906dff5 "><br>






 
# Task 2: Transfering encrypted file and decrypt it with hybrid encryption. 
**Question 1**:
Conduct transfering a file (deliberately choosen by you) between 2 computers. 
The file is symmetrically encrypted/decrypted by exchanging secret key which is encrypted using RSA. 
All steps are made manually with openssl at the terminal of each computer.

**Answer 1**:
## 1. Create a text file named `sender.txt`:
Firstly, create a txt file with a content inside. Then Save text file as .txt <br>

```sh
 echo "OMG I need higher score" > sender.txt 
```
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/1e58faf6-08c5-49da-9e3d-33ba91c9c16a "><br>

## 2: Create Public and Private Key in container receiver

Create RSA Private key and Public key
```sh
openssl genpkey -algorithm RSA -out private_key.pem -aes256
openssl rsa -pubout -in private_key.pem -out public_key.pem
```
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/0310720c-363b-49b7-9739-2db275f61142 "><br>

Then input the password and Enter. Complete creating pair of key
## 3. Using Nc to transfer public key from sender to receiver
At Sender input the command: 
```sh 
nc -l -p 12345 > public_key.pem
```
and the command for reciever:
```sh
cat public.pem | nc sender 12345
```
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/70d151e7-3ff0-467f-a3bc-1b7203c5f749"><br>

## 4. Create a random AES key to encryption

*Generate a random AES key*<br>

```sh
openssl rand -out secret.key 32
```

<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/c3129c77-981d-46f0-89dc-c9ffd26c733b"><br>


Encrypt the sender.txt file with AES-256 <br>
```sh
openssl enc -aes-256-cbc -salt -in sender.txt -out encrypted_file.bin -pass file:./secret.key
```


<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/d28aa27e-2f4c-4ee7-91f9-b23c1bae92f2"><br>


## 5. Using the public key (RSA) to enctypt the private key


Input both command  
```sh
 openssl rsautl -encrypt -inkey public_key.pem -pubin -in secret.key -out encrypted_key.bin
```
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/080d78f9-76f2-413f-b31b-5560d28eb4c6"><br>

## 6. Send the encrypted_file & encrypted_key from sender to receiver:
**Sender**
```sh
 cat encrypted_key.bin | nc receiver 12345
 cat encrypted_file.bin | nc receiver 123456
```
**Receiver**
```sh
nc -l -p 12345 > encrypted_key_copy.bin
 nc -l -p 123456 > encrypted_file_copy.bin
```
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/6bd04622-0d63-4724-a44f-5b33725ef104"><br>

## 7. The receiver uses the private key to decrypt the secret key and then uses the secret key to decrypt the sender.txt:

*Decrypted secret key*
```sh
openssl rsautl -decrypt -inkey private.pem -in encr
ypted_key_copy.bin -out decrypted_key.txt
```

<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/2e203c59-9352-44e8-96ab-ecd419ce5019">


*Decrypt the encrypted_file.bin file with the secret key*
```sh
openssl enc -d -aes-256-cbc -in encrypted_file.bin -out decrypted_plaintext.txt -pass file:decrypted_key.txt
```

<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/9ea92ae6-586e-43d1-a5da-470622de3dbd">

# Task 3: Firewall configuration
**Question 1**:
From VMs of previous tasks, install iptables and configure one of the 2 VMs as a web and ssh server. Demonstrate your ability to block/unblock http, icmp, ssh requests from the other host.

**Answer 1**:







