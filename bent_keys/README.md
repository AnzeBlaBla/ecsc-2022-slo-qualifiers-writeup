## Challenge Name: Bent Keys
Categories: Crypto, Web Security
Difficulty: Hard

## Description: 

Please start the vulnerable service from `RESOURCES`. This will start the `SSL/TLS` service on port `8443`. It is possible to get the private key from the server.

Once you have the private key, please run the following command

`openssl pkeyutl -derive -inkey private.key -peerkey public.key | sha1sum`

the `sha1` of the command above is the flag.

## Solution

After trying every possible exploit on Tomcat, we realized that it's probably just a crypto challenge (need to break the TLS encryption).

If you scan the service with [this tool](https://github.com/tls-attacker/TLS-Scanner) you can see it's vulnrable to the invalid curve exploit. You can use [this other tool](https://github.com/tls-attacker/TLS-Attacker) to do that exploit for you using the following command (after building it by following the instructions in the README):
`java -jar Attacks.jar invalid_curve -connect (YOUR DOCKER'S IP):8443 -executeAttack -renegotiation -additional_equations 0 -protocol_flows 1`

These arguments seem to work, but I can't really explain why (I guessed them until it worked :D ).

This should return the private key, which you then combine with the public key (which you can get from the server's certificate by using `openssl x509 -pubkey -noout -in server.crt > public.key`)

---
[Back to home](../README.md)
