
---

title: Dev Log #1 – Crafting

abstract: Hello and welcome to my first blog post. Over here, I'll just be posting a whole bunch of different content, ranging from projects, sports and hobbies. Hope you enjoy!

date: '2024-06-23'

banner: /static/modern-styling-in-react-banner.jpg

featured: true

---

As a personal project, I started working on building my own interpreter, following the book,  *[Crafting Interpreters](https://www.craftinginterpreters.com/welcome.html)* by Robert Nystrom. I plan to follow the book, implementing the Lox language, and eventually build my own programming language, full with compilers and/or interpreters. 

I enjoy working through the challenge problems in each chapter. I will now share my doubly linked list implementation in C. First I made the header file. Normally I write comments to clarify the specifications, but since I wrote this up quickly I didn't do that.

```c
#ifndef DOUBLY_LINKED_LIST_H

#define DOUBLY_LINKED_LIST_H

  

#include <stddef.h>

#include <stdlib.h>

#include <stdbool.h>

#include <string.h>

#include <assert.h>

#include <stdio.h>

  

typedef struct Node {

char *string;

struct Node* next;

struct Node* prev;

} Node;

  

typedef struct DoublyLinkedList {

Node* head;

Node* tail;

size_t size;

} DoublyLinkedList;

  

DoublyLinkedList* create_doubly_linked_list();

  

void insert_string_front(DoublyLinkedList* list, char* string);

  

void insert_string_back(DoublyLinkedList* list, char* string);

  

bool remove_string(DoublyLinkedList* list, char* string);

  

size_t get_size(DoublyLinkedList* list);

  

void *get_string_address(DoublyLinkedList* list, char* string);

  

void print_list(DoublyLinkedList* list);

  

void free_list(DoublyLinkedList* list);

  

Node *create_node(DoublyLinkedList* list, Node *prev, Node *next, char *string);

  

#endif // DOUBLY_LINKED_LIST_H
```

I normally just define my function declarations, structs and includes in my header file. Then for the actual implementation

```c
#include "linked_list.h"

  

DoublyLinkedList* create_doubly_linked_list() {

DoublyLinkedList* list = (DoublyLinkedList*)malloc(sizeof(DoublyLinkedList));

assert(list != NULL);

list->head = NULL;

list->tail = NULL;

list->size = 0;

return list;

}

  

void insert_string_front(DoublyLinkedList* list, char* string) {

assert(list);

create_node(list, NULL, list->head, string);

assert(list);

}

  

void insert_string_back(DoublyLinkedList* list, char* string) {

assert(list);

create_node(list, list->tail, NULL, string);

assert(list);

}

  

bool remove_string(DoublyLinkedList* list, char* string) {

bool removed = false;

Node *cur = list->head;

while(cur) {

if(strcmp(cur->string, string) == 0) {

removed = true;

  

Node *prev = cur->prev;

Node *next = cur->next;

  

prev ? prev->next = next : 0;

next ? next->prev = prev : 0;

list->head == cur ? list->head = next : 0;

list->tail == cur ? list->tail = prev : 0;

  

free(cur->string);

free((void*)cur);

  

list->size--;

  

break;

}

cur = cur->next;

}

return removed;

}

  

size_t get_size(DoublyLinkedList* list) {

return list->size;

}

  

void *get_string_address(DoublyLinkedList* list, char* string) {

Node *cur = list->head;

while(cur) {

if(strcmp(cur->string, string) == 0) {

return((void*)cur);

}

cur = cur->next;

}

return 0x0;

}

  

void print_list(DoublyLinkedList* list) {

Node* cur = list->head;

printf("Doubly Linked List [size: %zu]:\n", list->size);

while (cur) {

printf("[ %s ]", cur->string);

if (cur->next != NULL) {

printf(" <-> ");

}

cur = cur->next;

}

printf("\n");

}

  

// --- Helper --- //

  

Node *create_node(DoublyLinkedList* list, Node *prev, Node *next, char *string) {

Node *node = (Node *)malloc(sizeof(Node));

node->prev = prev;

node->next = next;

node->string = string;

  

if(prev == NULL) {

list->head ? list->head->prev = node : 0;

list->head = node;

}

  

if(next == NULL) {

list->tail ? list->tail->next = node : 0;

list->tail = node;

}

  

list->size++;

return node;

}

  

// Function to free the entire list

void free_list(DoublyLinkedList* list) {

Node* cur = list->head;

while (cur) {

Node* next = cur->next;

free(cur->string);

free(cur);

cur = next;

}

free(list);

}

  

int main() {

DoublyLinkedList* list = create_doubly_linked_list();

printf("Enter commands:\n");

  

char command[256];

char string[256];

  

while (true) {

printf("> ");

if (scanf("%255s", command) != 1) {

break;

}

  

if (strcmp(command, "insert_front") == 0) {

if (scanf("%255s", string) == 1) {

char *new_string = strdup(string);

insert_string_front(list, new_string);

} else {

printf("Invalid string.\n");

}

} else if (strcmp(command, "insert_back") == 0) {

if (scanf("%255s", string) == 1) {

char *new_string = strdup(string);

insert_string_back(list, new_string);

} else {

printf("Invalid string.\n");

}

} else if (strcmp(command, "remove") == 0) {

if (scanf("%255s", string) == 1) {

if (remove_string(list, string)) {

printf("String removed.\n");

} else {

printf("String not found.\n");

}

} else {

printf("Invalid string.\n");

}

} else if (strcmp(command, "print") == 0 || strcmp(command, "p") == 0) {

print_list(list);

} else if (strcmp(command, "size") == 0 || strcmp(command, "s") == 0) {

printf("Size of list: %zu\n", get_size(list));

} else if (strcmp(command, "exit") == 0) {

break;

} else {

printf("Unknown command.\n");

}

  

// Clear the input buffer to prevent issues with reading next command

int c;

while ((c = getchar()) != '\n' && c != EOF);

}

  

free_list(list);

return 0;

}
```

Finally to wrap it all up I wrote a makefile and initialized a git repository which I will link [here](https://github.com/treqo/c-linked-list).