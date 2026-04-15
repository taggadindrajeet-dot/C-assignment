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
