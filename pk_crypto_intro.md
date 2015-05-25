# Introduction to PK Crypto

##A. Ice-Cream Van Crypto

###1. Description
- In class, we saw how to make a “city map” of an ice-cream van problem by starting with the defining nodes (where the vans will be parked) and then drawing other neighboring nodes and paths from it. We also saw how we can make this into a rudimentary PK-cryptosystem by making our original map (with defining nodes highlighted) as the private key, and then redrawing the same map without defining nodes as the public map.
- Make another map-based PK-cryptosystem by yourself: starting with only three defining nodes for the private key (i.e., three van locations) but many other non-van nodes. Feel free to make as large a map as you want. Please show the private and public keys for this map, and verify that it works on a plain number input.

### 2. Submission

Submit PDF files (slides) showing your public and private key maps, and a demonstration of it encrypting and decrypting a number.

## B. Knapsack Cipher

### 1. Description

Fork the repo at: [https://github.com/ISS-Security/hw-knapsack_cipher.git](https://github.com/ISS-Security/hw-knapsack_cipher.git)

Modify the KnapsackCipher class to implement the two public functions to encrypt and decrypt:
- `encrypt(plaintext, superknap, m, n)`
- `decrypt(ciphertext, generalknap)`

The encrypt function should take a SuperKnapsack object and two prime numbers as the private key, and the decrypt function should take an array (general knapsack) as the public key. You may take advantage of the `ModularArithmetic.invert` method to find a modular inverse (e.g., `ModularArithmetic.invert(41, 491)` is `12`)

Test your code using `ruby spec/knapsack_spec.rb`

### 3. Submission

Submit your Github repo with full implementation
