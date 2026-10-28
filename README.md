## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## AIM:
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:


#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 100

void cleanText(char str[]) {
    int i, j = 0;

    for (i = 0; str[i]; i++) {
        if (isalpha((unsigned char)str[i])) {
            str[j++] = tolower((unsigned char)str[i]);
        }
    }
    str[j] = '\0';
}

void generateKeyTable(char key[], char table[5][5]) {
    int used[26] = {0};
    int row = 0, col = 0;

    used['j' - 'a'] = 1;

    for (int i = 0; key[i]; i++) {
        char ch = key[i];

        if (ch == 'j')
            ch = 'i';

        if (!used[ch - 'a']) {
            table[row][col++] = ch;
            used[ch - 'a'] = 1;

            if (col == 5) {
                col = 0;
                row++;
            }
        }
    }

    for (char ch = 'a'; ch <= 'z'; ch++) {
        if (!used[ch - 'a']) {
            table[row][col++] = ch;

            if (col == 5) {
                col = 0;
                row++;
            }
        }
    }
}

void prepareText(char plain[]) {
    char temp[SIZE];
    int i = 0, j = 0;

    while (plain[i] && j < SIZE - 2) {
        char a = plain[i];
        char b = plain[i + 1];

        temp[j++] = a;

        if (b == '\0') {
            temp[j++] = (a == 'x') ? 'q' : 'x';
            i++;
        } else if (a == b) {
            temp[j++] = (a == 'x') ? 'q' : 'x';
            i++;
        } else {
            temp[j++] = b;
            i += 2;
        }
    }

    temp[j] = '\0';
    strcpy(plain, temp);
}

void search(char table[5][5], char a, char b, int pos[]) {
    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (table[i][j] == a) {
                pos[0] = i;
                pos[1] = j;
            }
            if (table[i][j] == b) {
                pos[2] = i;
                pos[3] = j;
            }
        }
    }
}

void encrypt(char plain[], char table[5][5]) {
    int pos[4];

    for (int i = 0; plain[i] && plain[i + 1]; i += 2) {
        search(table, plain[i], plain[i + 1], pos);

        if (pos[0] == pos[2]) {
            plain[i] = table[pos[0]][(pos[1] + 1) % 5];
            plain[i + 1] = table[pos[2]][(pos[3] + 1) % 5];
        } else if (pos[1] == pos[3]) {
            plain[i] = table[(pos[0] + 1) % 5][pos[1]];
            plain[i + 1] = table[(pos[2] + 1) % 5][pos[3]];
        } else {
            char c1 = table[pos[0]][pos[3]];
            char c2 = table[pos[2]][pos[1]];

            plain[i] = c1;
            plain[i + 1] = c2;
        }
    }
}

int main() {
    char key[SIZE], plain[SIZE];
    char table[5][5];

    printf("Enter Key: ");
    fgets(key, SIZE, stdin);
    key[strcspn(key, "\n")] = '\0';

    printf("Enter Plaintext: ");
    fgets(plain, SIZE, stdin);
    plain[strcspn(plain, "\n")] = '\0';

    cleanText(key);
    cleanText(plain);

    prepareText(plain);
    generateKeyTable(key, table);

    printf("\nKey       : %s\n", key);
    printf("Plaintext : %s\n", plain);

    printf("\nKey Matrix:\n");
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++)
            printf("%c ", table[i][j]);
        printf("\n");
    }

    encrypt(plain, table);

    printf("\nCiphertext: %s\n", plain);

    return 0;
}




## Output:
<img width="1825" height="682" alt="image" src="https://github.com/user-attachments/assets/e61625b7-12ce-4aea-b457-90109723c41e" />

**RESULT:**

The Playfair Cipher encryption technique was implemented successfully in C. The plaintext was encrypted using the key entered by the user, and the corresponding ciphertext was displayed.

