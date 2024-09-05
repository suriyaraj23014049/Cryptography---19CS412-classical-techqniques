# Cryptography---19CS412-classical-techqniques

# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
// Function to perform Caesar Cipher encryption
void caesarEncrypt(char *text, int key) {
 for (int i = 0; text[i] != '\0'; i++) {
 char c = text[i];
 // Check if the character is an uppercase letter
 if (c >= 'A' && c <= 'Z') {
 text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
 }
 // Check if the character is a lowercase letter
 else if (c >= 'a' && c <= 'z') {
 text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
 }
 // Ignore non-alphabetic characters
 }
}
// Function to perform Caesar Cipher decryption
void caesarDecrypt(char *text, int key) {
 // Decryption is the same as encryption with a negative key
 caesarEncrypt(text, -key);
}
int main() {
 char message[100]; // Declare a character array to store the message
 int key;
 printf("Enter the message to encrypt: ");
 fgets(message, sizeof(message), stdin); // Read input from the user
 printf("Enter the Caesar Cipher key (an integer): ");
 scanf("%d", &key); // Read the key from the user
 // Encrypt the message using the Caesar Cipher
 caesarEncrypt(message, key);
 printf("Encrypted Message: %s", message);
 // Decrypt the message back to the original
 caesarDecrypt(message, key);
 printf("Decrypted Message: %s", message);
 return 0;
}
```
## OUTPUT:
![image](https://github.com/user-attachments/assets/cf7dbdad-a00a-4b98-813f-82ceb79f3f5f)



## RESULT:
The program is executed successfully

---------------------------------


# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To develop a simple C program to implement PlayFair Cipher.

## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
 ```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyTable(char key[], int keyLength, char keyTable[SIZE][SIZE]) {
    int used[26] = {0};  // To track used characters
    int i, j, k = 0;

    for (i = 0; i < keyLength; i++) {
        if (key[i] != 'j') {
            if (used[key[i] - 'a'] == 0) {
                used[key[i] - 'a'] = 1;
                keyTable[k / SIZE][k % SIZE] = key[i];
                k++;
            }
        }
    }

    for (i = 0; i < 26; i++) {
        if (i + 'a' != 'j' && used[i] == 0) {
            keyTable[k / SIZE][k % SIZE] = i + 'a';
            k++;
        }
    }
}

void search(char keyTable[SIZE][SIZE], char a, char b, int *row1, int *col1, int *row2, int *col2) {
    int i, j;

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                *row1 = i;
                *col1 = j;
            } else if (keyTable[i][j] == b) {
                *row2 = i;
                *col2 = j;
            }
        }
    }
}

void encryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + 1) % SIZE];
        *y = keyTable[row2][(col2 + 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + 1) % SIZE][col1];
        *y = keyTable[(row2 + 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void decryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + SIZE - 1) % SIZE];
        *y = keyTable[row2][(col2 + SIZE - 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + SIZE - 1) % SIZE][col1];
        *y = keyTable[(row2 + SIZE - 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void encrypt(char keyTable[SIZE][SIZE], char plainText[], char cipherText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; plainText[i] != '\0'; i += 2) {
        a = plainText[i];
        if (plainText[i + 1] != '\0') {
            b = plainText[i + 1];
        } else {
            b = 'x';
        }

        if (a == b) {
            b = 'x';
            i--;
        }

        encryptPair(keyTable, a, b, &cipherText[j], &cipherText[j + 1]);
        j += 2;
    }
    cipherText[j] = '\0';
}

void decrypt(char keyTable[SIZE][SIZE], char cipherText[], char plainText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; cipherText[i] != '\0'; i += 2) {
        a = cipherText[i];
        b = cipherText[i + 1];

        decryptPair(keyTable, a, b, &plainText[j], &plainText[j + 1]);
        j += 2;
    }
    plainText[j] = '\0';
}

int main() {
    char key[100], plainText[100], cipherText[100], decryptedText[100];
    char keyTable[SIZE][SIZE];
    int i, keyLength;

    printf("Enter the key: ");
    scanf("%s", key);

    for (i = 0; key[i] != '\0'; i++) {
        key[i] = tolower(key[i]);
    }

    keyLength = strlen(key);
    generateKeyTable(key, keyLength, keyTable);

    printf("Enter the plaintext: ");
    scanf("%s", plainText);

    for (i = 0; plainText[i] != '\0'; i++) {
        plainText[i] = tolower(plainText[i]);
    }

    encrypt(keyTable, plainText, cipherText);
    printf("Encrypted Text: %s\n", cipherText);

    decrypt(keyTable, cipherText, decryptedText);
    printf("Decrypted Text: %s\n", decryptedText);

    return 0;
}

```
## OUTPUT:
![image](https://github.com/user-attachments/assets/8c1c5895-b238-44b9-bd4e-db89bb1aa75b)





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

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Function to encode a triplet of characters
void encode(char a, char b, char c, char ret[]) {
    int x, y, z;
    int posa = (int)a - 65;
    int posb = (int)b - 65;
    int posc = (int)c - 65;

    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];

    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

// Function to decode a triplet of characters
void decode(char a, char b, char c, char ret[]) {
    int x, y, z;
    int posa = (int)a - 65;
    int posb = (int)b - 65;
    int posc = (int)c - 65;

    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];

    ret[0] = key[(x % 26 < 0) ? (26 + x % 26) : (x % 26)];
    ret[1] = key[(y % 26 < 0) ? (26 + y % 26) : (y % 26)];
    ret[2] = key[(z % 26 < 0) ? (26 + z % 26) : (z % 26)];
    ret[3] = '\0';
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;

    printf("Enter the message to encode: ");
    fgets(msg, 1000, stdin);
    msg[strcspn(msg, "\n")] = '\0'; // Remove the newline character from input

    printf("Input message : %s\n", msg);

    // Convert the input message to uppercase
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Remove spaces
    n = strlen(msg) % 3;

    // Append padding text 'X' if necessary
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message : %s\n", msg);

    // Encode the message
    for (int i = 0; i < strlen(msg); i += 3) {
        char a = msg[i];
        char b = msg[i + 1];
        char c = msg[i + 2];
        char encoded[4];
        encode(a, b, c, encoded);
        strcat(enc, encoded);
    }

    printf("Encoded message : %s\n", enc);

    // Decode the message
    for (int i = 0; i < strlen(enc); i += 3) {
        char a = enc[i];
        char b = enc[i + 1];
        char c = enc[i + 2];
        char decoded[4];
        decode(a, b, c, decoded);
        strcat(dec, decoded);
    }

    printf("Decoded message : %s\n", dec);
    return 0;
}

```
## OUTPUT:
![image](https://github.com/user-attachments/assets/2d9efc48-fbb6-4be7-add3-d90560ffbf85)


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

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>  // For exit() function
#include <ctype.h>   // For toupper() function
#include <string.h>  // For strlen() function

void encipher();
void decipher();

int main() {
    int choice;
    while (1) {
        printf("\n1. Encrypt Text");
        printf("\n2. Decrypt Text");
        printf("\n3. Exit");
        printf("\n\nEnter Your Choice: ");
        scanf("%d", &choice);

        if (choice == 3)
            exit(0);
        else if (choice == 1)
            encipher();
        else if (choice == 2)
            decipher();
        else
            printf("Please Enter a Valid Option.\n");
    }
    return 0;  // Added return statement for the main function
}

void encipher() {
    unsigned int i, j;
    char input[50], key[10];

    printf("\n\nEnter Plain Text: ");
    scanf("%s", input);  // Removed newline for better input handling

    printf("Enter Key Value: ");
    scanf("%s", key);

    printf("Resultant Cipher Text: ");
    for (i = 0, j = 0; i < strlen(input); i++, j++) {
        if (j >= strlen(key)) {
            j = 0;
        }
        printf("%c", 65 + (((toupper(input[i]) - 65) + (toupper(key[j]) - 65)) % 26));
    }
    printf("\n");  // Added newline for output formatting
}

void decipher() {
    unsigned int i, j;
    char input[50], key[10];
    int value;

    printf("\n\nEnter Cipher Text: ");
    scanf("%s", input);  // Removed newline for better input handling

    printf("Enter the Key Value: ");
    scanf("%s", key);

    printf("Resultant Plain Text: ");
    for (i = 0, j = 0; i < strlen(input); i++, j++) {
        if (j >= strlen(key)) {
            j = 0;
        }

        // Calculate the decrypted character value
        value = (toupper(input[i]) - 65) - (toupper(key[j]) - 65);
        if (value < 0) {
            value += 26;  // Correct for negative values in circular shift
        }
        printf("%c", 65 + (value % 26));
    }
    printf("\n");  // Added newline for output formatting
}
```
## OUTPUT:
![image](https://github.com/user-attachments/assets/d04d2999-2279-4176-a03e-6c1e2a92ede7)



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

## PROGRAM:
```
#include <stdio.h>
#include <string.h>

void encryptRailFence(char *text, int key, char *cipherText) {
    int len = strlen(text);
    int row, col, direction;
    char rail[key][len];

    // Initializing the rail matrix with null characters
    for (row = 0; row < key; row++)
        for (col = 0; col < len; col++)
            rail[row][col] = '\n';

    // Placing characters in the rail matrix in a zig-zag manner
    row = 0;
    direction = 1; // 1 for down, -1 for up
    for (col = 0; col < len; col++) {
        rail[row][col] = text[col];
        if (row == 0)
            direction = 1;
        else if (row == key - 1)
            direction = -1;
        row += direction;
    }

    // Reading the matrix row-wise to get the cipher text
    int index = 0;
    for (row = 0; row < key; row++) {
        for (col = 0; col < len; col++) {
            if (rail[row][col] != '\n') {
                cipherText[index++] = rail[row][col];
            }
        }
    }
    cipherText[index] = '\0';
}

void decryptRailFence(char *cipherText, int key, char *plainText) {
    int len = strlen(cipherText);
    int row, col, direction;
    char rail[key][len];

    // Initializing the rail matrix with null characters
    for (row = 0; row < key; row++)
        for (col = 0; col < len; col++)
            rail[row][col] = '\n';

    // Marking the places in the rail matrix where the cipher text characters will go
    row = 0;
    direction = 1; // 1 for down, -1 for up
    for (col = 0; col < len; col++) {
        rail[row][col] = '*';
        if (row == 0)
            direction = 1;
        else if (row == key - 1)
            direction = -1;
        row += direction;
    }

    // Filling the rail matrix with the cipher text characters
    int index = 0;
    for (row = 0; row < key; row++) {
        for (col = 0; col < len; col++) {
            if (rail[row][col] == '*' && index < len) {
                rail[row][col] = cipherText[index++];
            }
        }
    }

    // Reading the matrix in a zig-zag manner to get the plain text
    row = 0;
    direction = 1; // 1 for down, -1 for up
    for (col = 0; col < len; col++) {
        plainText[col] = rail[row][col];
        if (row == 0)
            direction = 1;
        else if (row == key - 1)
            direction = -1;
        row += direction;
    }
    plainText[len] = '\0';
}

int main() {
    char text[100], cipherText[100], plainText[100];
    int key;

    // Input the plain text
    printf("Enter the plain text: ");
    gets(text);

    // Input the key (number of rails)
    printf("Enter the key (number of rails): ");
    scanf("%d", &key);

    // Encrypt the plain text
    encryptRailFence(text, key, cipherText);
    printf("Encrypted Text: %s\n", cipherText);

    // Decrypt the cipher text
    decryptRailFence(cipherText, key, plainText);
    printf("Decrypted Text: %s\n", plainText);

    return 0;
}
```
## OUTPUT:
![image](https://github.com/user-attachments/assets/127a3f29-20eb-41b4-b9ec-6e26e02806a1)


## RESULT:
The program is executed successfully
