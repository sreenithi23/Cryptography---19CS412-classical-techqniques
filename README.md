# Caeser Cipher
Caeser Cipher using with different key values
### NAME: SREENITHI E

### REG:212223220109
### DATE:18.3.25
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
```
PROGRAM:
#include <stdio.h>
#include <string.h>
#include <ctype.h>
void main()

{
    char plain[10],cipher[10];
    int key,i,length;
    int result;
    printf("\n Enter the plain text:");
    scanf("%s", plain);
    printf("\n Enter the key value:");
    scanf("%d", &key);
    printf("\n \n \t PLAIN TEXt: %s", plain);
    printf("\n \n \t ENCRYPTED TEXT:");
    for(i=0, length = strlen(plain); i<length; i++)
    {
        
        cipher[i]=plain[i] + key;
        if (isupper(plain[i]) && (cipher[i] > 'Z'))
        cipher[i] = cipher[i] - 26;
        if (islower(plain[i]) && (cipher[i] > 'z'))
        cipher[i] = cipher[i] - 26;
        printf("%c", cipher[i]);

    }
    printf("\n \n \t AFTER DECRYPTION : ");
    for(i=0;i<length;i++)
    {
        
        plain[i]=cipher[i]-key;
        if(isupper(cipher[i])&&(plain[i]<'A'))
        plain[i]=plain[i]+26;
        if(islower(cipher[i])&&(plain[i]<'a'))
        plain[i]=plain[i]+26;
        printf("%c",plain[i]);
    }
    getchar();

    
}
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/b8eeb476-c632-4aa1-9b63-94f838ac9a10)


## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values
## NAME:SREENITHI E
## REG: 212223220109

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

#define SIZE 5

void generateKeyMatrix(char key[], char matrix[SIZE][SIZE]) {
    int alpha[26] = {0};
    int i, j, k = 0;
    char current;

    for (i = 0; key[i] != '\0'; i++) {
        current = toupper(key[i]);
        if (current == 'J') current = 'I';
        if (current < 'A' || current > 'Z' || alpha[current - 'A'])
            continue;
        alpha[current - 'A'] = 1;
        key[k++] = current;
    }
    key[k] = '\0';

    i = 0; k = 0;
    for (int row = 0; row < SIZE; row++) {
        for (int col = 0; col < SIZE; col++) {
            if (i < strlen(key)) {
                matrix[row][col] = key[i++];
            } else {
                for (char ch = 'A'; ch <= 'Z'; ch++) {
                    if (ch == 'J' || alpha[ch - 'A'])
                        continue;
                    matrix[row][col] = ch;
                    alpha[ch - 'A'] = 1;
                    break;
                }
            }
        }
    }
}

void findPosition(char matrix[SIZE][SIZE], char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';  
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void processDigraph(char a, char b, char matrix[SIZE][SIZE], char *resA, char *resB, int encrypt) {
    int row1, col1, row2, col2;
    findPosition(matrix, a, &row1, &col1);
    findPosition(matrix, b, &row2, &col2);

    if (row1 == row2) {  
        *resA = matrix[row1][(col1 + (encrypt ? 1 : SIZE - 1)) % SIZE];
        *resB = matrix[row2][(col2 + (encrypt ? 1 : SIZE - 1)) % SIZE];
    } else if (col1 == col2) {  
        *resA = matrix[(row1 + (encrypt ? 1 : SIZE - 1)) % SIZE][col1];
        *resB = matrix[(row2 + (encrypt ? 1 : SIZE - 1)) % SIZE][col2];
    } else {  // Rectangle swap
        *resA = matrix[row1][col2];
        *resB = matrix[row2][col1];
    }
}

void preprocessText(char *text) {
    char temp[100] = {0};
    int k = 0;

    for (int i = 0; text[i]; i++) {
        if (isalpha(text[i])) {
            temp[k++] = toupper(text[i] == 'J' ? 'I' : text[i]);
        }
    }
    temp[k] = '\0';
    strcpy(text, temp);
}

void encryptDecryptText(char *text, char matrix[SIZE][SIZE], int encrypt) {
    preprocessText(text);
    char result[100] = {0};
    int len = strlen(text), k = 0;

    for (int i = 0; i < len; i += 2) {
        char a = text[i];
        char b = (i + 1 < len) ? text[i + 1] : 'X';

        if (a == b) {
            b = 'X';  
            i--;      
        }

        char resA, resB;
        processDigraph(a, b, matrix, &resA, &resB, encrypt);
        result[k++] = resA;
        result[k++] = resB;
    }
    result[k] = '\0';
    strcpy(text, result);
}

void printMatrix(char matrix[SIZE][SIZE]) {
    printf("KEY MATRIX:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    char key[100], text[100], matrix[SIZE][SIZE];
    
    printf("ENTER THE KEY: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    printf("ENTER TEXT TO ENCRYPT: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = '\0';

    generateKeyMatrix(key, matrix);
    printMatrix(matrix);

    encryptDecryptText(text, matrix, 1);  
    printf("ENCRYPTED TEXT: %s\n", text);

    encryptDecryptText(text, matrix, 0);  
    printf("DECRYPTED TEXT: %s\n", text);

    return 0;
}
```
``

## OUTPUT:

Output:

![image](https://github.com/user-attachments/assets/f9c205e5-cfef-4897-b920-7c01c2ce4f6d)

Key text: Monarchy Plain text: instruments Cipher text: gatlmzclrqtx

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values
## NAME:SREENITHI E
## REG: 212223220109

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
#include <ctype.h>

#define SIZE 2  // Size of the key matrix (2x2 for simplicity)

int keyMatrix[SIZE][SIZE];

void toUpperCase(char *str) {
    for (int i = 0; str[i]; i++) {
        str[i] = toupper(str[i]);
    }
}

void removeSpaces(char *str) {
    int count = 0;
    for (int i = 0; str[i]; i++) {
        if (str[i] != ' ') {
            str[count++] = str[i];
        }
    }
    str[count] = '\0';
}

void getKeyMatrix(char *key) {
    int k = 0;
    toUpperCase(key);
    removeSpaces(key);
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            keyMatrix[i][j] = key[k++] - 'A';
        }
    }
}

void encrypt(char *text, char *cipher) {
    toUpperCase(text);
    removeSpaces(text);
    int len = strlen(text);
    if (len % SIZE != 0) {
        text[len++] = 'X';  // Padding if needed
        text[len] = '\0';
    }
    
    for (int i = 0; i < len; i += SIZE) {
        for (int row = 0; row < SIZE; row++) {
            int sum = 0;
            for (int col = 0; col < SIZE; col++) {
                sum += keyMatrix[row][col] * (text[i + col] - 'A');
            }
            cipher[i + row] = (sum % 26) + 'A';
        }
    }
    cipher[len] = '\0';
}

void printKeyMatrix() {
    printf("Key Matrix:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%d ", keyMatrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    char key[SIZE * SIZE + 1], text[100], cipher[100];
    
    printf("Enter key (4 letters): ");
    scanf("%s", key);
    getKeyMatrix(key);
    printKeyMatrix();
    
    printf("Enter plaintext: ");
    scanf("%s", text);
    
    encrypt(text, cipher);
    printf("Ciphertext: %s\n", cipher);
    
    return 0;
}

```
``
## OUTPUT:


![image](https://github.com/user-attachments/assets/bc9140a8-e4c6-4208-b952-e8b687e784b9)

Input Message : SecurityLaboratory


Padded Message : SECURITYLABORATORY Encrypted Message : EACSDKLCAEFQDUKSXU Decrypted Message : SECURITYLABORATORY


## RESULT:

The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values
### NAME: SREENITHI E
### REG NO: 212223220109

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
#include <ctype.h>
#include <string.h>
#include <stdlib.h>

void encipher();
void decipher();

int main() {
    int choice;
    while (1) {
        printf("\n1. ENCRYPT TEXT");
        printf("\t2. DECRYPED TEXT");
        printf("\t3. EXIT");
        printf("\n\nENTER YOUR CHOICE: ");
        scanf("%d", &choice);
        getchar(); // Consume newline character after scanf
        
        if (choice == 3)
        { 
            printf("\nEXIT\n");
            return 0;
        }
        else if (choice == 1)
            encipher();
        else if (choice == 2)
            decipher();
        else
            printf("PLEASE ENTER A VALID OPTION.\n");
    }
}

void encipher() {
    unsigned int i, j;
    char input[50], key[10];
    printf("\nENCRYPTION\n");

    printf("\nENTER PLAIN TEXT: ");
    scanf("%49s", input); // Prevent buffer overflow

    printf("\nENTER KEY VALUE: ");
    scanf("%9s", key); // Prevent buffer overflow

    printf("\nRESULTANT CIPHER TEXT: ");
    for (i = 0, j = 0; i < strlen(input); i++, j++) {
        if (j >= strlen(key)) {
            j = 0; // Reset key index if it exceeds the key length
        }
        printf("%c", 65 + (((toupper(input[i]) - 65) + (toupper(key[j]) - 65)) % 26)); // Encryption formula
    }
    printf("\n"); // New line after output
}

void decipher() {
    unsigned int i, j;
    char input[50], key[10];
    int value;
    printf("\nDECRYPTION\n");

    printf("\nENTER CIPHER TEXT: ");
    scanf("%49s", input); // Prevent buffer overflow

    printf("\nENTER THE KEY VALUE: ");
    scanf("%9s", key); // Prevent buffer overflow

    printf("\nDECRYPTED PLAIN TEXT: ");
    for (i = 0, j = 0; i < strlen(input); i++, j++) {
        if (j >= strlen(key)) {
            j = 0; // Reset key index if it exceeds the key length
        }
        // Decryption formula
        value = (toupper(input[i]) - 65) - (toupper(key[j]) - 65);
        if (value < 0) {
            value += 26; // Correct the negative wrap-around in the alphabet
        }
        printf("%c", 65 + (value % 26));
    }
    printf("\n"); // New line after output
}
```
## OUTPUT:



![image](https://github.com/user-attachments/assets/8062ffac-c7b0-4623-bab7-3b767bca1150)


## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values
## NAME:SREENITHI
## REG:212223220109

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
 int main() {
 int i, j, k, l;
 char a[20], c[20], d[20];
 printf("\n\t\tRAIL FENCE TECHNIQUE\n");
 printf("\nEnter the input string: ");
 fgets(a, sizeof(a), stdin);
 a[strcspn(a, "\n")] = '\0';
 l = strlen(a); 
 for (i = 0, j = 0; i < l; i++) {
 if (i % 2 == 0) {
 c[j++] = a[i];
 }
 }
 for (i = 0; i < l; i++) {
 if (i % 2 == 1) {
 c[j++] = a[i];
 }
 }
 c[j] = '\0'; 
 printf("\nCipher text after applying rail fence: %s\n", c);
 
 if (l % 2 == 0) {
 k =l / 2;
 } else {
 k =(l / 2) + 1;
 }
 
 for (i = 0, j = 0; i < k; i++) {
 d[j] = c[i];
 j += 2;
 }
 for (i = k, j = 1; i < l; i++) {
 d[j] = c[i];
 j += 2;
 }
 d[l] = '\0'; 
 printf("\nText after decryption: %s\n", d);
 return 0; 
 }
 ```
## OUTPUT:


![Screenshot 2025-04-12 214531](https://github.com/user-attachments/assets/ffe9180f-4321-489b-b135-9dd13f856684)


## RESULT:
The program is executed successfully
