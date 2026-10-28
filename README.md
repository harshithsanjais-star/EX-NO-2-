## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
~~~
## Name : HARSHITH SANJAI S 
## Reg. No. : 212225040121
~~~
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
char matrix[5][5];  
void createMatrix(char key[])  
{  
    int used[26] = {0};  
    int i, j, k = 0;  
    char ch;  
    used['J' - 'A'] = 1;  
    for (i = 0; key[i] != '\0'; i++)  
    {  
        ch = toupper((unsigned char)key[i]);  
        if (ch >= 'A' && ch <= 'Z')  
        {  
            if (ch == 'J')  
                ch = 'I';  
            if (!used[ch - 'A'])  
            {  
                matrix[k / 5][k % 5] = ch;  
                used[ch - 'A'] = 1;  
                k++;  
            }  
        }  
    }  
    for (ch = 'A'; ch <= 'Z'; ch++)  
    {  
        if (!used[ch - 'A'])  
        {  
            matrix[k / 5][k % 5] = ch;  
            k++; 
        }  
    }  
}  
void prepareText(char text[], char result[])  
{  
    char temp[200];  
    int i, n = 0, k = 0;  
    for (i = 0; text[i] != '\0'; i++)  
    {  
        char ch = toupper((unsigned char)text[i]);  
        if (ch >= 'A' && ch <= 'Z')  
        { 
            if (ch == 'J')  
                ch = 'I';  
            temp[n++] = ch;  
        }  
    }  
    for (i = 0; i < n;)  
    {  
        char a = temp[i++];  
        char b;  
        if (i >= n || a == temp[i])  
        {  
            b = (a == 'X') ? 'Q' : 'X';  
        }  
        else  
        {  
            b = temp[i++];  
        }  
        result[k++] = a;  
        result[k++] = b;  
    }  
    result[k] = '\0';  
}  
void findPosition(char ch, int *row, int *col)  
{  
    int i, j; 
    if (ch == 'J')  
        ch = 'I';    
    for (i = 0; i < 5; i++)  
    {  
        for (j = 0; j < 5; j++)  
        {  
            if (matrix[i][j] == ch)  
            {  
                *row = i;  
                *col = j;  
                return;  
            }  
        }  
    }  
}  
void encrypt(char text[])  
{  
    int i, r1, c1, r2, c2;    
    for (i = 0; text[i] != '\0'; i += 2)  
    {  
        findPosition(text[i], &r1, &c1);  
        findPosition(text[i + 1], &r2, &c2);  
        if (r1 == r2)  
        {  
            text[i] = matrix[r1][(c1 + 1) % 5];  
            text[i + 1] = matrix[r2][(c2 + 1) % 5];  
        }  
        else if (c1 == c2)  
        {  
            text[i] = matrix[(r1 + 1) % 5][c1];  
            text[i + 1] = matrix[(r2 + 1) % 5][c2];  
        }  
        else  
        {  
            text[i] = matrix[r1][c2];  
            text[i + 1] = matrix[r2][c1];  
        }  
    }  
}  
void decrypt(char text[])  
{  
    int i, r1, c1, r2, c2;    
    for (i = 0; text[i] != '\0'; i += 2)  
    {  
        findPosition(text[i], &r1, &c1);  
        findPosition(text[i + 1], &r2, &c2);    
        if (r1 == r2)  
        {  
            text[i] = matrix[r1][(c1 + 4) % 5];  
            text[i + 1] = matrix[r2][(c2 + 4) % 5];  
        }  
        else if (c1 == c2)  
        {  
            text[i] = matrix[(r1 + 4) % 5][c1];  
            text[i + 1] = matrix[(r2 + 4) % 5][c2];  
        }  
        else  
        {  
            text[i] = matrix[r1][c2];  
            text[i + 1] = matrix[r2][c1];  
        }  
    }  
} 
int main()  
{  
    char key[100], PT[200], CT[200], DT[200];  
    printf("Enter the key: ");  
    fgets(key, sizeof(key), stdin);  
    printf("Enter the plaintext: ");  
    fgets(PT, sizeof(PT), stdin);  
    createMatrix(key);  
    prepareText(PT, CT);  
    printf("\nPlayfair Matrix:\n");  
    for (int i = 0; i < 5; i++)  
    {  
        for (int j = 0; j < 5; j++)  
            printf("%c ", matrix[i][j]);  
printf("\n");  
}  
encrypt(CT);  
printf("\nEncrypted Text: %s\n", CT);  
strcpy(DT, CT);  
decrypt(DT);  
printf("Decrypted Text: %s\n", DT);  
return 0;  
}

## Output:
<img width="922" height="602" alt="image" src="https://github.com/user-attachments/assets/8b8ae25b-e978-4f36-9e8e-57f6840d0c99" />

## Result:
Thus the implementation of Playfair Substitution technique had been executed successfully.
