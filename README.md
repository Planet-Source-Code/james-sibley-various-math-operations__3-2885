<div align="center">

## Various math operations


</div>

### Description

Factorial, list prime numbers to a given amount, factor a positive integer, matrix multiplication (the fun part), and test to see if the inputed value was a number and not letters or excess decimal points/neg. signs.
 
### More Info
 
numbers the user wants to include in the math operation

EXE should be easy to understand, the code is a bit different... understand character arrays!!!

all functions are void except the notvalid function. this function will return whether ot not the inputed value was a valid number.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[James Sibley](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/james-sibley.md)
**Level**          |Advanced
**User Rating**    |3.8 (15 globes from 4 users)
**Compatibility**  |C, C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+, UNIX C\+\+
**Category**       |[Math](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/math__3-12.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/james-sibley-various-math-operations__3-2885/archive/master.zip)

### API Declarations

```
#include <iostream>
#include <fstream>
#include <windows.h>
#include <math.h>
#include <cstdlib>
#include <iomanip>
```


### Source Code

```
// James Sibley
// 17-19 October 2001
// Period 6
// Made up problem.... solved :P
#include <iostream>
#include <fstream>
#include <windows.h>
#include <math.h>
#include <cstdlib>
#include <iomanip>
using namespace std;
/* Functions that will be used */
void listprimes(); // Make a list of prime numbers
void factor(); // Factor a number or let you know that it is prime
void quadratic(); // Use the quadratic equation
void matrix();   // Matrix multiplication
void factorial(); // Find the factorial of a number
bool notvalid(char[]); // Did the user input a correct number?
/* Stucture used to hold the name and address of the functions */
struct m
{
	char name[256]; // Char to hold the name that will appear on the menu
	void (*list)(); // Pointer to a function
} menu[] = {    // Creating an array of data type "m"
	"Factorial", factorial, // Name to appear on menu and address of function
	"List Prime numbers", listprimes,
	"Factor a number", factor,
	"Quadratic formula", quadratic,
	"Matrix multiplication", matrix
};
void main(){ // Void main - do not return a value
	int size = sizeof(menu)/sizeof(m); // Find the # of items to include on menu
	int x; // loop variable
	char input[64] = "-1"; // Input variable for the menu
	bool exit = false; // Looping condition
	while(!exit) {
		// If the user entered anything other than the list provides or
		// The number was less than 0, keep looping
		while(atoi(input) > size || atoi(input) < 0) {
			system("cls"); // Clear the screen
			cout << "Select a number from the list\n";
			for(x = 0; x < 80; x++) // Output 80 *s which will cover the
				cout << "*";    // width of the screen
			// Output the menu
			// menu[x].name means that we are selecting an item from an
			// array of a structure. This array holds key information.
			// This information is the name to include in the outputed
			// menu and the address of the function to call if that item
			// is selected... :)
			for(x = 0; x < size; x++)
			{
				cout << x+1 << ": " << menu[x].name << "\n";
			}
			cout << "0: Exit\n";
			for(x = 0; x < 80; x++) // Output more *s...
				cout << "*";
			cout << "[?]: "; // This is the prompt the user will see
			cin.getline(input,64);	 // The user will input a selection from the menu
			if(notvalid(input) || input[0] == '\0') {
				strcpy(input, "-1");
				continue;
			}
		}
		if(strcmp(input,"0") == 0) {
				exit = true;
				break;
		// If the user picked something on the list other than zero,
		// the choice will be evaluated here. Here, we will call
		// a function through its address found in the array we created
		// for the menu
		}else{
				cout << "\n";
				(*menu[atoi(input) - 1].list)();
		}
		system("pause");
		strcpy(input, "-1");
	}
}
/* This function will list prime numbers to a user-inputed amount */
void listprimes() {
	int x, y; char SIZE[64] = {1}; // loop variables... except SIZE..
					// SIZE will be the size of a bool array
	bool *prime;  // pointer to a future list of prime numbers
	char save[512]; // Where the user feels like saving the file...
	while(notvalid(SIZE) || atoi(SIZE) < 2) {
		cout << "Enter a valid number: ";
		cin.getline(SIZE,64);  // prime numbers 1 to SIZE
	}
	cout << "Enter the path/file to save the file: ";
	cin >> save;  // Save the file... HERE!
	prime = new bool[atoi(SIZE)]; // Create a bool array based on SIZE
							// If you imput a number in this array
							// later on, it will return whether or
							// not it is prime... neato!
	// We are going to assume for the moment that all the numbers
	// are prime... only because i said so...
	for(x = 1; x <= atoi(SIZE); x++)
		prime[x-1] = true;
	prime[0] = false; // 1 is not a prime number
	/* calculate prime numbers */
	// This is the neat part. Here we are going to determine
	// all the numbers that are not prime. This is done by dividing
	// all numbers by 2 to the square root of (SIZE). If a number is
	// found to be divisible, then the number is said to not be prime
	// and prime[x-1] = false. All numbers left being "true" are prime
	// numbers.
	for(x = 4; x <= atoi(SIZE); x++){
		for(y = 2; y < (sqrt(x)+1); y++)
			if(x%y == 0){
				prime[x-1] = false;
				break;} // break out of the loop if a composite number
	}					// is found... !!!!! This saves time by not
						// having to loop through meaningless numbers
	/* end calulation */
	ofstream primefile(save, ios::trunc); // creating a file to save
										 // the list
	primefile << "Prime numbers: 1 - " << atoi(SIZE) << "\n"; // outputting
												    // to the file
	// This will output to the file ONLY the prime numbers by looking
	// for those in the bool array that remain true... sweet!
	cout << "\n";
	for(x = 1; x <= atoi(SIZE); x++)
		if(prime[x-1] == true)
			primefile << x << "\n";
	// For fun... a message box in Windows (WinBlowz, hehehe)...
	MessageBox(NULL, "Complete! List of prime numbers has been created.\n\n:)",
		    "Done", MB_OK | 48);
	delete [] prime;
}
/* this function will factor a number. If it is prime, it will inform you. */
void factor(void) {
	char number[64] = {0}; // The number that they enter
	long remainder; // remainder after we divide a divisible number
	long *factors; // all the factors of a number go here
	long counter = 0; // the number of factors in a number
	long x; // looping variable
	for(;;) { // infinite loop
		strcpy(number,"8388609");
		while(atoi(number) > 8388607 || atoi(number) < 2 || notvalid(number))
		{
			cout << "[Enter number to test factor, 0 to stop, up to 8388607]: ";
			cin.getline(number,64);
			if(atoi(number) == 0)
				return;
		}
		// Make an array half the size a the inputed number.
		// This is because if all the factors are 2, then the
		// number of factors will be half of that number
		factors = new long[atoi(number) / 2];
		remainder = atoi(number);
		long half = atoi(number) / 2 + 1;
		long temp; // the remainder of the remainder divided by
				  // a tested number
		counter = 0;
		/* find factors */
		// all this peice of code does is divide it
		// by 2 to half the number... if it is divisible
		// by x... then that is a factor and x is set back
		// to 2. Factors[counter] will hold the factors
		for(x = 2; x <= atoi(number); x++) {
			temp = remainder%x;
			if(temp == 0) {
				factors[counter] = x;
				counter++;
				remainder /= x;
				x = 1;
			}
		}
		/* End find factors */
		factors[counter] = 0;
		// If the counter went through one time, then
		// the number is prime. If the number is not prime,
		// it will display every prime factor of the
		// inputed number
		if(counter == 1) {
				cout << "Prime number";
				factors[0] = 0;
		}else {
			x = 0;
			while(factors[x]){
				cout << factors[x] << " ";
				x++; }
			}
			cout << "\n";
		}
	delete [] factors; // Delete the array
}
/* This function will find the x intercepts and */
/* tell what the vertext is           */
void quadratic() {
	char a[64] = {1}, b[64] = {1}, c[64] = {1}; // numbers used in ax^2 + bx + c
	bool inter; // true if y can equal zero
	double first, second; // the first and second intercepts
	char pre[64] = {1}; // the decimal precision
	cout << "For ax^2 + bx + c, enter in the following pieces of information\n";
	while(notvalid(a)) {
		cout << "a = ";
		cin.getline(a,64);
	}
	// "a" cannot be zero because if it was, it would not be a parabola
	// and you would be dividing by zero. anything divided by zero is
	// so huge that it is undefined...
	while(atoi(a) == 0 || notvalid(a)) {
			if(atof(a) == 0) cout << "\'a\' may not equal 0!\n";
			cout << "a = ";
			cin.getline(a,64);
	}
	while(notvalid(b)) {
		cout << "b = ";
		cin.getline(b,64);
	}
	while(notvalid(c)) {
		cout << "c = ";
		cin.getline(c,64);
	}
	while(notvalid(pre)) {
		cout << "Enter the decimal precision: ";
		cin.getline(pre,64);
	}
	cout << "\n";
	for(int x = 0; x < 80; x++)
		cout << "*";
	// Find the value of the disciminant (spelling?).
	// I am not sure if the "Unknown error!" part will ever execute,
	// but it could be possible... so i just included it to prevent
	// a program crash during unknown errors (which does happen)
	double dis = atof(b) * atof(b) - 4 * atof(a) * atof(c);
	if(dis < 0){
		cout << "X-intercepts lie on the imaginary-axis!\n";
		inter = false;
	}else if(dis == 0) {
		cout << "Vertex lies on x-axis!\n";
		inter = true;
	}else if(dis > 0) {
		cout << "Two points cross the x-axis!\n";
		inter = true;
	}else {
		cout << "Unknown Error!\n";
		inter = false;
	}
	// If the parabola does cross the x-axis atleast once,
	// then the answers are evaluated here...
	if(inter) {
		cout << "X-intercepts: (";
		first = (-1 * atof(b) + sqrt(dis))/(2 * atof(a));
		cout << setprecision(atoi(pre)) << first << ",0) - (";
		second = (-1 * atof(b) - sqrt(dis))/(2 * atof(a));
		cout << setprecision(atoi(pre)) << second << ",0)\n";
	}
	// The vertex of a parabola is equal to (-b)/(2a)
	// for the x value. To find the y value, just plug
	// x in
	double vertex = (-1 * atof(b)) / (2 * atof(a));
	cout << "Vertex: (" << setprecision(atoi(pre)) << vertex
		 << "," << setprecision(atoi(pre)) << atof(a)*vertex*vertex + atof(b) * vertex + atof(c) << ")\n";
	for(x = 0; x < 80; x++)
		cout << "*";
}
/* This function will multiply to matrices together. */
void matrix()
{
	// These are the dimensions of the two
	// matrices...
	char dim1[64] = "0", dim2[64] = "0", dima1[64] = "0", dima2[64] = "0";
	// prompting the user to enter enter valid dimensions
	while(atoi(dim1) == 0 || atoi(dim2) == 0 || notvalid(dim1) || notvalid(dim2))
	{
		cout << "Enter the first dimension for matrix one: ";
		cin.getline(dim1,64);
		cout << "Enter the second dimension for matrix one: ";
		cin.getline(dim2,64);
	}
	// Not only does the user have to enter valid dimension
	// for the second matrix... but the first one has
	// to be the same as the last one of the first matrix...
	// if not, multiplication will not be possible
	while(atoi(dima1) == 0 || atoi(dima2) == 0 || atoi(dima1) != atoi(dim2) || notvalid(dima1) || notvalid(dima2))
	{
		cout << "Enter the first dimension for matrix two: ";
		cin.getline(dima1,64);
		cout << "Enter the second dimension for matrix two: ";
		cin.getline(dima2,64);
	}
	// Initalizing three, 2500-entry each matrices...
	double matrix1[50][50] = {{0},{0}};
	double matrix2[50][50] = {{0},{0}};
	double pmatrix[50][50] = {{0},{0}};
	char temp[64] = {1};
	// Ask the user to enter in the values for the first matrix
	// and then storing the values.
	for(int x = 0; x < atoi(dim1); x++) {
		for(int y = 0; y < atoi(dim2); y++)
		{
				cout << "Enter \'" << x+1 << y+1 << "\' for matrix one: ";
				cin.getline(temp,64);
				if(notvalid(temp)) {
					y--;
					continue;
				}else{
					matrix1[x][y] = atof(temp);
					strcpy(temp, "\x01");
				}
		}
	}
	// Ask the user to enter in the values for the second matrix
	// and then storing the values.
	for(x = 0; x < atoi(dima1); x++) {
		for(int y = 0; y < atoi(dima2); y++)
		{
			cout << "Enter \'" << x+1 << y+1 << "\' for matrix two: ";
			cin.getline(temp,64);
			if(notvalid(temp)) {
				y--;
				continue;
			}else{
				matrix2[x][y] = atof(temp);
				strcpy(temp, "\x01");
			}
		}
	}
	// This is where the matrices are multiplied together.
	for(x = 0; x < atoi(dim1); x++) {
		for(int y = 0; y < atoi(dima2); y++) {
			for(int z = 0; z < atoi(dim2); z++) {
					pmatrix[x][y] += matrix1[x][z]*matrix2[z][y];
			}
		}
	}
	// This code will display the results of the multiplication
	for(x = 0; x < atoi(dim1); x++) {
		cout << "\n[";
		for(int y = 0; y < atoi(dima2); y++) {
			cout << pmatrix[x][y];
			if((y + 1) != atoi(dima2))
				cout << " ";
		}
		cout << "]";
	}
	cout << "\n";
}
/* This function will find the factorial of a number */
void factorial()
{
	char number[64] = {1}; // the number the user inputs
	double long hold;
	while(atol(number) < 0 || notvalid(number)) {
		cout << "Enter a number for the factorial operation: ";
		cin.getline(number,64);
		// Check to see if the user selected a number that is
		// not zero or negative
		if(atol(number) < 0 || notvalid(number)) {
			cout << "Not Valid!\n";
		}
	}
	if(atol(number) == 0 || atol(number) == 1) {
		cout << "\n" << atol(number) << "! = 1\n";
		return;
	}
	hold = (long)atof(number);
	double long total = 1;
	// factorial a number
	// (can factorial be used as a verb? :P )
	for(double long x = hold; x >= 1; x--) {
		total *= x;
	}
	cout << "\n" << setprecision(20) << atol(number) << "! = " << total << "\n";
}
/* this is determine if a string contains valid numbers and parts */
bool notvalid(char input[])
{
	int x = 0, decCounter = 0, negCount = 0, cond;
	if(strcmp(input, "-") == 0 || strcmp(input, ".") == 0)
		return true;
	while(input[x]) {
		cond = (input[x] >= '0' && input[x] <= '9') || input[x] == '.' || input[x] == '-';
		if(!cond)
			return true;
		x++;
	}
	x = 0;
	while(input[x]) {
		if(input[x] == '.')
			decCounter++;
		else if(input[x] == '-')
			negCount++;
		if(decCounter > 1 || negCount > 1)
			return true;
		x++;
	}
	x = 1;
	while(input[x]) {
		if(input[x] == '-')
			return true;
		x++;
	}
	return false;
}
```

