# C-assignment
// Library managment system

#include<bits/stdc++.h>
using namespace std;

// Book Node (Linked List)
class Book {
public:
    int id;
    string title;
    bool issued;
    Book* next;

    Book(int id, string title) {
        this->id = id;
        this->title = title;
        issued = false;
        next = NULL;
    }
};

class Library {
    Book* head;

    queue<int> waitingList;   // Queue
    stack<int> recentReturn;  // Stack

public:
    Library() {
        head = NULL;
    }

    // Add Book
    void addBook(int id, string title) {
        Book* newBook = new Book(id, title);

        if(head == NULL) {
            head = newBook;
        } else {
            Book* temp = head;
            while(temp->next != NULL)
                temp = temp->next;
            temp->next = newBook;
        }

        cout << "Book Added Successfully\n";
    }

    // Display Books
    void displayBooks() {
        Book* temp = head;
        while(temp != NULL) {
            if(!temp->issued)
                cout << temp->id << " - " << temp->title << endl;
            temp = temp->next;
        }
    }

    // Search Book
    void searchBook(int id) {
        Book* temp = head;
        while(temp != NULL) {
            if(temp->id == id) {
                cout << "Book Found: " << temp->title << endl;
                return;
            }
            temp = temp->next;
        }
        cout << "Book Not Found\n";
    }

    // Issue Book
    void issueBook(int id) {
        Book* temp = head;
        while(temp != NULL) {
            if(temp->id == id) {
                if(!temp->issued) {
                    temp->issued = true;
                    cout << "Book Issued Successfully\n";
                } else {
                    cout << "Book not available, added to waiting list\n";
                    waitingList.push(id);
                }
                return;
            }
            temp = temp->next;
        }
        cout << "Book Not Found\n";
    }

    // Return Book
    void returnBook(int id) {
        Book* temp = head;
        while(temp != NULL) {
            if(temp->id == id) {
                temp->issued = false;
                recentReturn.push(id);

                cout << "Book Returned\n";

                // Check waiting list
                if(!waitingList.empty() && waitingList.front() == id) {
                    cout << "Book issued to next person in waiting list\n";
                    waitingList.pop();
                    temp->issued = true;
                }
                return;
            }
            temp = temp->next;
        }
    }

    // Show Recently Returned (Stack)
    void showRecentReturns() {
        stack<int> temp = recentReturn;
        cout << "Recently Returned Books: ";
        while(!temp.empty()) {
            cout << temp.top() << " ";
            temp.pop();
        }
        cout << endl;
    }
};

int main() {
    Library lib;
    int choice, id;
    string title;

    do {
        cout << "\n1.Add Book\n2.Issue Book\n3.Return Book\n4.Search Book\n5.Display Books\n6.Recent Returns\n0.Exit\n";
        cin >> choice;

        switch(choice) {
            case 1:
                cout << "Enter ID and Title: ";
                cin >> id;
                cin.ignore();
                getline(cin, title);
                lib.addBook(id, title);
                break;

            case 2:
                cout << "Enter Book ID: ";
                cin >> id;
                lib.issueBook(id);
                break;

            case 3:
                cout << "Enter Book ID: ";
                cin >> id;
                lib.returnBook(id);
                break;

            case 4:
                cout << "Enter Book ID: ";
                cin >> id;
                lib.searchBook(id);
                break;

            case 5:
                lib.displayBooks();
                break;

            case 6:
                lib.showRecentReturns();
                break;
        }

    } while(choice != 0);

    return 0;
}


1. Introduction

The Library Management System is a software application designed to manage books efficiently in a library. This project is implemented using Data Structures in C++, specifically Linked List, Stack, and Queue.

The system allows users to:

Add new books
Issue books
Return books
Search for books
Display available books

Additionally, it uses:

Queue to manage waiting lists
Stack to track recently returned books

This makes the system more efficient and organized.

2. Objectives of the Project

The main objectives of this project are:

To understand the use of Linked List for dynamic data storage
To implement Stack and Queue in a real-life application
To manage library operations like issuing and returning books
To improve programming and problem-solving skills
To simulate a real-world library system
3. Methodology / System Design
Data Structures Used:
Linked List
Used to store book records
Each node contains:
Book ID
Title
Issued status
Pointer to next book
Queue (FIFO - First In First Out)
Used for waiting list of books
If a book is already issued, the request is added to queue
Stack (LIFO - Last In First Out)
Used to track recently returned books
Last returned book is shown first
Working of System:
Add Book: Inserts a new book at the end of the linked list
Issue Book:
If available → issue immediately
If not → add to waiting queue
Return Book:
Marks book as available
Pushes book ID into stack
Issues to next waiting user if exists
Search Book: Finds book using ID
Display Books: Shows all available (not issued) books
4. Implementation / Source Code

The project is implemented in C++ using Object-Oriented Programming.

Main Components:
Class Book
Stores book details (ID, title, issued status)
Uses pointer for linked list
Class Library
Manages all operations:
addBook()
issueBook()
returnBook()
searchBook()
displayBooks()
showRecentReturns()
Main Function
Provides menu-driven interface
Takes user input and performs operations
Key Features in Code:
Dynamic memory allocation using new
Use of STL:
queue for waiting list
stack for recent returns
Loop-based menu system for user interaction
5. Conclusion

This project successfully demonstrates the practical use of Data Structures in solving real-world problems.

By using:

Linked List → efficient book storage
Queue → fair waiting system
Stack → tracking recent returns

The system becomes efficient and user-friendly.

Overall, this project helps in understanding how different data structures can be combined to build a complete application.
