# RSA-encrypted-messenger

Initially, the aim of this project was to implement the RSA encryption scheme. Completing that, I then wrote a TCP instant messaging program that made use of my RSA implementation to allow for users to send encrypted messages to each other.

## Features

### Instant messaging
Users can send plaintext messages to other users connected to the running central server (`server.java`)
### Encrypted messaging
Users have the option of encrypting their messages (RSA encryption scheme). Ciphertext is generated clientside
and delivered to the recipient by the central server.
### Digital Signatures
If a user decides to encrypt their message, digital signatures will automatically used to enforce the integrity of the message during transit across the network. It also provides a means of authentication for the recipient; the recipient can authenticate the sender, if they can decrypt the digital signature with the senders public key.

## Quickstart guide
1. Start `server.java` on a host machine
2. Configure `client.java` to connect to the server running on the host machine by altering the values on
**Strong** __lines 6 and 7__ in `RSA.java`. By default `client.java` is set to connect to a server instance running `localhost`
port 25565
3. Run instances of `client.java` for each user.
4. Initially, you will need to setup values for public key pair (n,e). You will be prompted for values for n , p and e. Details for the conditions of these values are provided in the program and examples are provided below:
```
p = 97 , q = 101 , e = 251
p = 11 , q = 13 , e = 23
```
*  p and q must be large prime numbers. e must be relatively prime to p , q and (p-1)\*(q-1); 1 < e < (p-1)\*(q-1)
5. Finally, use the help command on client instances for an explanation of the uses of each command

## Future work / improvements
* Currently, `server.java` stores connected clients in a linked list, giving search performance O(n) time complexity. This can be improved by using a large hash map (with linked lists at each cell), giving search times of O(1) time complexity in best and average cases but O(n) time complexity in worst cases (e.g. a very large number of clients are connected at once)
* Key generation is a very manual process that could ideally be automated for the user.
* Protection against replay attacks; When a message is intercepted by an attacker, copied and sent to the recipient. Later on, the attacker resends the copied message to the recipient. The digital signature of the message is still encrypted with the original senders private key allowing the attacker to pose as the original sender. This could be prevented by time stamping encrypted messages - messages expire after a set amount of time and rejected. Another solution is to get the recipient to send to a token to the sender and have them encrypt it with their private key and send it back. If the recipient can decrypt the token with public key of the sender then the sender is authenticated.
* Implement mutual authentication to prevent similar attacks such as replay attacks
* User interaction is not optimal and could improved by creating a frontend GUI. 
