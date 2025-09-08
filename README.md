# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
PROGRAM:
CaearCipher.

```

#include <stdio.h>
#include <ctype.h>  

void encrypt(char text[], int key) {
    for (int i = 0; text[i] != '\0'; i++) {
        char ch = text[i];
        if (isupper(ch)) {
            text[i] = ((ch - 'A' + key) % 26) + 'A';
        } else if (islower(ch)) {
            text[i] = ((ch - 'a' + key) % 26) + 'a';
        }
    }
}

void decrypt(char text[], int key) {
    for (int i = 0; text[i] != '\0'; i++) {
        char ch = text[i];
        if (isupper(ch)) {
            text[i] = ((ch - 'A' - key + 26) % 26) + 'A';
        } else if (islower(ch)) {
            text[i] = ((ch - 'a' - key + 26) % 26) + 'a';
        }
    }
}

int main() {
    char text[100];
    int key;

    printf("Enter a message: ");
    scanf("%[^\n]s", text);

    printf("Enter the key (shift): ");
    scanf("%d", &key);

    encrypt(text, key);
    printf("Encrypted: %s\n", text);

    decrypt(text, key);
    printf("Decrypted: %s\n", text);

    return 0;
}

```
## OUTPUT:
OUTPUT:
Simulating Caesar Cipher

<img width="362" height="247" alt="Screenshot (442)" src="https://github.com/user-attachments/assets/8b3d2a67-64a5-4f9e-a751-c2d6dad6c61b" />


Input : Anna University
Encrypted Message : Dqqd Xqlyhuvlwb Decrypted Message : Anna University

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:

```

#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyMatrix[5][5];

void generateKeyMatrix(char key[]) {
    int i, j, k = 0, used[26] = {0};

    used['J' - 'A'] = 1;

    for (i = 0; key[i]; i++) {
        char ch = toupper(key[i]);
        if (ch < 'A' || ch > 'Z') continue;
        if (!used[ch - 'A']) {
            keyMatrix[k/5][k%5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }
    for (i = 0; i < 26; i++) {
        if (!used[i]) {
            keyMatrix[k/5][k%5] = 'A' + i;
            k++;
        }
    }
}

void printMatrix() {
    printf("Key Matrix:\n");
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            printf("%c ", keyMatrix[i][j]);
        }
        printf("\n");
    }
}

void findPosition(char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I'; // Treat I and J same
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyMatrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encryptPair(char a, char b, char *x, char *y) {
    int r1, c1, r2, c2;
    findPosition(a, &r1, &c1);
    findPosition(b, &r2, &c2);

    if (r1 == r2) { // Same row
        *x = keyMatrix[r1][(c1+1)%5];
        *y = keyMatrix[r2][(c2+1)%5];
    } else if (c1 == c2) { // Same col
        *x = keyMatrix[(r1+1)%5][c1];
        *y = keyMatrix[(r2+1)%5][c2];
    } else { // Rectangle rule
        *x = keyMatrix[r1][c2];
        *y = keyMatrix[r2][c1];
    }
}

void encrypt(char plaintext[], char ciphertext[]) {
    int i, j = 0;
    for (i = 0; plaintext[i]; i++) {
        if (!isalpha(plaintext[i])) continue;
        plaintext[j++] = toupper(plaintext[i]);
    }
    plaintext[j] = '\0';

    char prepared[200]; j = 0;
    for (i = 0; plaintext[i]; i++) {
        prepared[j++] = plaintext[i];
        if (plaintext[i+1] && plaintext[i] == plaintext[i+1])
            prepared[j++] = 'X';
    }
    if (j % 2 != 0) prepared[j++] = 'X';
    prepared[j] = '\0';

    int k = 0;
    for (i = 0; i < j; i += 2) {
        char x, y;
        encryptPair(prepared[i], prepared[i+1], &x, &y);
        ciphertext[k++] = x;
        ciphertext[k++] = y;
    }
    ciphertext[k] = '\0';
}

int main() {
    char key[100], plaintext[100], ciphertext[200];

    printf("Enter key: ");
    scanf("%s", key);

    generateKeyMatrix(key);
    printMatrix();

    printf("Enter plaintext: ");
    scanf("%s", plaintext);

    encrypt(plaintext, ciphertext);
    printf("Ciphertext: %s\n", ciphertext);

    return 0;
}

```

## OUTPUT:
Output:
Key text: Monarchy Plain text: instruments Cipher text: gatlmzclrqxa

<img width="371" height="312" alt="Screenshot (443)" src="https://github.com/user-attachments/assets/ae9de1c9-52f0-4b6d-b5e7-676087d3b175" />



## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:

```
#include <stdio.h>
#include <string.h>

int charToNum(char c) {
    return c - 'A';
}

char numToChar(int n) {
    return (n % 26) + 'A';
}


void encrypt(char plaintext[], int key[2][2], char ciphertext[]) {
    int i, p[2], c[2], len = strlen(plaintext), k = 0;

    if (len % 2 != 0) {
        plaintext[len] = 'X';
        plaintext[len+1] = '\0';
        len++;
    }

    for (i = 0; i < len; i += 2) {
        p[0] = charToNum(plaintext[i]);
        p[1] = charToNum(plaintext[i+1]);

        c[0] = (key[0][0]*p[0] + key[0][1]*p[1]) % 26;
        c[1] = (key[1][0]*p[0] + key[1][1]*p[1]) % 26;

        ciphertext[k++] = numToChar(c[0]);
        ciphertext[k++] = numToChar(c[1]);
    }
    ciphertext[k] = '\0';
}

int main() {
    char plaintext[100], ciphertext[100];
    int key[2][2];

    printf("Enter plaintext (UPPERCASE): ");
    scanf("%s", plaintext);

    printf("Enter 2x2 key matrix:\n");
    for (int i = 0; i < 2; i++)
        for (int j = 0; j < 2; j++)
            scanf("%d", &key[i][j]);

    encrypt(plaintext, key, ciphertext);

    printf("Ciphertext: %s\n", ciphertext);

    return 0;
}

```

## OUTPUT:

Simulating Hill Cipher

<img width="427" height="233" alt="Screenshot (444)" src="https://github.com/user-attachments/assets/04e515fa-976a-4878-94c0-c50eb151d872" />

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:

```

#include <stdio.h>
#include <string.h>
#include <ctype.h>

void generateKey(char key[], char newKey[], int len) {
    int keyLen = strlen(key);
    for (int i = 0; i < len; i++) {
        newKey[i] = toupper(key[i % keyLen]);
    }
    newKey[len] = '\0';
}

void encrypt(char plaintext[], char key[], char ciphertext[]) {
    int len = strlen(plaintext);
    char newKey[100];
    generateKey(key, newKey, len);

    for (int i = 0; i < len; i++) {
        char p = toupper(plaintext[i]);
        ciphertext[i] = ((p - 'A' + (newKey[i] - 'A')) % 26) + 'A';
    }
    ciphertext[len] = '\0';
}

void decrypt(char ciphertext[], char key[], char plaintext[]) {
    int len = strlen(ciphertext);
    char newKey[100];
    generateKey(key, newKey, len);

    for (int i = 0; i < len; i++) {
        char c = toupper(ciphertext[i]);
        plaintext[i] = ((c - 'A' - (newKey[i] - 'A') + 26) % 26) + 'A';
    }
    plaintext[len] = '\0';
}

int main() {
    char plaintext[100], key[100], ciphertext[100], decrypted[100];

    printf("Enter plaintext (UPPERCASE): ");
    scanf("%s", plaintext);

    printf("Enter key: ");
    scanf("%s", key);

    encrypt(plaintext, key, ciphertext);
    printf("Ciphertext: %s\n", ciphertext);

    decrypt(ciphertext, key, decrypted);
    printf("Decrypted text: %s\n", decrypted);

    return 0;
}

```
## OUTPUT:

Simulating Vigenere Cipher

<img width="421" height="206" alt="Screenshot (445)" src="https://github.com/user-attachments/assets/c57551da-21a3-460d-8656-c8844fabdb5f" />


## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

```

#include <stdio.h>
#include <string.h>
#include <stdbool.h>

void encryptRailFence(char text[], int key, char result[]) {
    int len = strlen(text);
    char rail[key][len];
    bool dir_down = false;
    int row = 0, col = 0;

  
    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            rail[i][j] = '\n';

    
    for (int i = 0; i < len; i++) {
        if (row == 0 || row == key - 1)
            dir_down = !dir_down;
        rail[row][col++] = text[i];
        row += dir_down ? 1 : -1;
    }


    int idx = 0;
    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            if (rail[i][j] != '\n')
                result[idx++] = rail[i][j];
    result[idx] = '\0';
}

void decryptRailFence(char cipher[], int key, char result[]) {
    int len = strlen(cipher);
    char rail[key][len];
    bool dir_down;
    int row, col;

    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            rail[i][j] = '\n';

    dir_down = false;
    row = 0; col = 0;
    for (int i = 0; i < len; i++) {
        if (row == 0) dir_down = true;
        if (row == key - 1) dir_down = false;
        rail[row][col++] = '*';
        row += dir_down ? 1 : -1;
    }


    int index = 0;
    for (int i = 0; i < key; i++)
        for (int j = 0; j < len; j++)
            if (rail[i][j] == '*' && index < len)
                rail[i][j] = cipher[index++];

    result[len] = '\0';
    row = 0; col = 0;
    dir_down = false;
    for (int i = 0; i < len; i++) {
        if (row == 0) dir_down = true;
        if (row == key - 1) dir_down = false;
        result[i] = rail[row][col++];
        row += dir_down ? 1 : -1;
    }
}

int main() {
    char text[100], cipher[100], decrypted[100];
    int key;

    printf("Enter plaintext: ");
    scanf("%[^\n]", text);

    printf("Enter key (number of rails): ");
    scanf("%d", &key);

    encryptRailFence(text, key, cipher);
    printf("Ciphertext: %s\n", cipher);

    decryptRailFence(cipher, key, decrypted);
    printf("Decrypted text: %s\n", decrypted);

    return 0;
}

```
## OUTPUT:

<img width="361" height="196" alt="Screenshot (446)" src="https://github.com/user-attachments/assets/222f5757-2ad0-4092-8d98-a89f8ea4eb20" />


## RESULT:
The program is executed successfully
