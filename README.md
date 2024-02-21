1.Implementation of stack using array

#include <stdio.h>
#define MAX_SIZE 100

int stack[MAX_SIZE];
int top = -1;

void push(int item) {
    if (top == MAX_SIZE - 1) {
        printf("Stack Overflow! Unable to push item.\n");
        return;
    }
    stack[++top] = item;
}

int pop() {
    if (top == -1) {
        printf("Stack Underflow! Unable to pop item.\n");
        return -1;
    }
    return stack[top--];
}

int is_empty() {
    return top == -1;
}

int peek() {
    if (top == -1) {
        printf("Stack is empty!\n");
        return -1;
    }
    return stack[top];
}

int size() {
    return top + 1;
}

int main() {
    // Example usage of the stack
    push(10);
    push(20);
    push(30);
    
    printf("Current size of the stack: %d\n", size());
    printf("Top item of the stack: %d\n", peek());
    
    printf("Popped item: %d\n", pop());
    printf("Popped item: %d\n", pop());
    
    printf("Current size of the stack: %d\n", size());
    
    return 0;
}


2.Conversion of infix expression to postfix expression
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Function to return precedence of operators
int prec(char c) {
	if (c == '^')
		return 3;
	else if (c == '/' || c == '*')
		return 2;
	else if (c == '+' || c == '-')
		return 1;
	else
		return -1;
}

// Function to return associativity of operators
char associativity(char c) {
	if (c == '^')
		return 'R';
	return 'L'; // Default to left-associative
}

// The main function to convert infix expression to postfix expression
void infixToPostfix(char s[]) {
	char result[1000];
	int resultIndex = 0;
	int len = strlen(s);
	char stack[1000];
	int stackIndex = -1;

	for (int i = 0; i < len; i++) {
		char c = s[i];

		// If the scanned character is an operand, add it to the output string.
		if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')) {
			result[resultIndex++] = c;
		}
		// If the scanned character is an ‘(‘, push it to the stack.
		else if (c == '(') {
			stack[++stackIndex] = c;
		}
		// If the scanned character is an ‘)’, pop and add to the output string from the stack
		// until an ‘(‘ is encountered.
		else if (c == ')') {
			while (stackIndex >= 0 && stack[stackIndex] != '(') {
				result[resultIndex++] = stack[stackIndex--];
			}
			stackIndex--; // Pop '('
		}
		// If an operator is scanned
		else {
			while (stackIndex >= 0 && (prec(s[i]) < prec(stack[stackIndex]) ||
									prec(s[i]) == prec(stack[stackIndex]) &&
										associativity(s[i]) == 'L')) {
				result[resultIndex++] = stack[stackIndex--];
			}
			stack[++stackIndex] = c;
		}
	}

	// Pop all the remaining elements from the stack
	while (stackIndex >= 0) {
		result[resultIndex++] = stack[stackIndex--];
	}

	result[resultIndex] = '\0';
	printf("%s\n", result);
}

// Driver code
int main() {
	char exp[] = "a+b*(c^d-e)^(f+g*h)-i";

	// Function call
	infixToPostfix(exp);

	return 0;
}



3.Evaluation of expressions
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX_SIZE 100

struct Stack {
    int stack[MAX_SIZE];
    int top;
};

void initialize(struct Stack* s) {
    s->top = -1;
}

void push(struct Stack* s, int item) {
    if (s->top == MAX_SIZE - 1) {
        printf("Stack Overflow! Unable to push item.\n");
        return;
    }
    s->stack[++s->top] = item;
}

int pop(struct Stack* s) {
    if (s->top == -1) {
        printf("Stack Underflow! Unable to pop item.\n");
        return 0;
    }
    return s->stack[s->top--];
}

int evaluate_postfix(char* postfix) {
    struct Stack stack;
    initialize(&stack);
    int i, operand1, operand2, result;
    
    for (i = 0; postfix[i] != '\0'; i++) {
        if (isdigit(postfix[i])) {
            push(&stack, postfix[i] - '0');
        } else {
            operand2 = pop(&stack);
            operand1 = pop(&stack);
            
            switch (postfix[i]) {
                case '+':
                    result = operand1 + operand2;
                    break;
                case '-':
                    result = operand1 - operand2;
                    break;
                case '*':
                    result = operand1 * operand2;
                    break;
                case '/':
                    result = operand1 / operand2;
                    break;
                case '^':
                    result = pow(operand1, operand2);
                    break;
                default:
                    printf("Invalid operator encountered.\n");
                    return 0;
            }
            
            push(&stack, result);
        }
    }
    
    return pop(&stack);
}

int main() {
    char postfix[MAX_SIZE];
    printf("Enter a postfix expression: ");
    scanf("%s", postfix);
    
    int result = evaluate_postfix(postfix);
    printf("The result is: %d\n", result);
    
    return 0;
    }


    4.Tower of Hanoi is a mathematical puzzle where we have three rods
and n disks. The objective of the puzzle is to move the entire stack to another rod, obeying
the following simple rules

#include <stdio.h>

void towerOfHanoi(int n, char source, char destination, char auxiliary) {
    if (n == 1) {
        printf("Move disk 1 from %c to %c\n", source, destination);
        return;
    }
    towerOfHanoi(n-1, source, auxiliary, destination);
    printf("Move disk %d from %c to %c\n", n, source, destination);
    towerOfHanoi(n-1, auxiliary, destination, source);
}

int main() {
    int numDisks = 3;
    towerOfHanoi(numDisks, 'A', 'C', 'B');
    return 0;
}
