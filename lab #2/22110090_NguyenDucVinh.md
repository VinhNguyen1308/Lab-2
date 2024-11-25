# Lab #2,22110090, Nguyen Duc Vinh, INSE330380E_01FIE
# Task 1: Transfer files between computers


**Question 1**: Conduct transfering a single plaintext file between 2 computers, 
Using openssl to implementing measures manually to ensure file integerity and authenticity at sending side, 
then veryfing at receiving side.

**Answer 1**:
## 1. Create a text file named `plain.txt`:
### Step 1:  Create two containers to emulate two computers
Create two Ubuntu containers to act as the sender and receiver.
<br>
  **Pull image Ubuntu** <br>
  `
  docker pull ubuntu:latest
` 
<br>
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/4f3769c7-4ec6-4669-adc2-e17565e04f87"><br>

### Step 2: Create containers for "sender" and "receiver"
 <br>

  `
 docker run -it --name sender ubuntu:latest ` <br>
 `
 docker run -it --name receiver ubuntu:latest
` 
 <br>


<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/2d1daac9-5cb6-4390-8da3-5f05020dd2b4 "><br>

 ### Step 3: After creating 2 containers "sender" and "reciever", check it if its work or not
 
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/093719e9-c653-4fde-9686-1e5aeb2f7784 "><br>

### Step 4: Install OpenSSL in container

Open terminal for each container (sender và receiver) and install OpenSSL:

```sh
apt update
apt install -y openssl tar
```
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/2a195766-5995-4765-ba2b-33ba538e1136 "><br>

### Step 5: Create file and perform digital signature operations at "sender" container
<br> 

**Create a text file named** `plain.txt`
<br> 
```sh
echo "This is a secure message." > plaintext.txt
```
<br>

**Create a key pair and digital signature** <br>
Create RSA key pair Create private key and public key:
<br>
```sh
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048

openssl rsa -in private_key.pem -pubout -out public_key.pem

```
Create a digital signature Use a private key to sign the `plaintext.txt` file.
```sh
openssl dgst -sha256 -sign private_key.pem -out signature.bin plaintext.txt
```
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/db4fab29-f271-4b36-a3ac-6ca822b4ea29"><br>

Check the Digital Signature: 
<br>

<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/229dda1d-623e-4d34-ab03-5b1d4d8ab45c"><br>

Package the original file, signature, and public key into a `.tar` file
<br>
```sh
tar -cvf package.tar plaintext.txt signature.bin public_key.pem
```
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/00c2cac7-562c-493e-a5b5-15942216a1fe"><br>

### Step 6: Move the file from the "sender" container to the "receiver" container.
Copy the file from the "sender" container to the host machine on the external terminal.<br>
<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/8d81c8ff-5f95-4401-ae58-37b54428bfcd"><br>


Copy file from the server into the "receiver" container

<img width="500" alt="Screenshot" src="https://github.com/user-attachments/assets/923a5091-eadf-4398-af74-15c82c8eeb1b"><br>

### Step 7: Extract file from "reciever"

`tar -xvf package.tar`
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/1ff87348-a453-493a-851d-c62963b9520a "><br>

### Step 8: Check integrity and authentication Use public keys and signatures to verify the file content
<img width="500" alt="Screenshot" src=" https://github.com/user-attachments/assets/2fadaf40-ec26-4c40-80e3-0d557768f649"><br>

This means that the file has not been altered and its authenticity is guaranteed.

# Task 2: Transfering encrypted file and decrypt it with hybrid encryption. 
**Question 1**:
Conduct transfering a file (deliberately choosen by you) between 2 computers. 
The file is symmetrically encrypted/decrypted by exchanging secret key which is encrypted using RSA. 
All steps are made manually with openssl at the terminal of each computer.

# Task 3: Firewall configuration
**Question 1**:
From VMs of previous tasks, install iptables and configure one of the 2 VMs as a web and ssh server. Demonstrate your ability to block/unblock http, icmp, ssh requests from the other host.

**Answer 1**:







