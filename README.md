# Programing-Fundamental-Library-Management-System-
First Semester Project
#include <iostream>
#include <string>
using namespace std;

struct TBook {
    string name, author, category, location;
    bool status; // true = available, false = issued
    int issuedto = 0;
};

TBook AddSingleBook() {
    TBook book;
    cout << "Enter category of the book: ";
    cin >> book.category;
    cout << "Enter title of the book: ";
    cin >> book.name;
    cout << "Enter author's name: ";
    cin >> book.author;
    cout << "Enter location of the book:\n1. Rack 1\n2. Rack 2\n3. Rack 3\n";
    int choice;
    cin >> choice;

    switch (choice) {
    case 1: book.location = "Rack 1"; break;
    case 2: book.location = "Rack 2"; break;
    case 3: book.location = "Rack 3"; break;
    default:
        book.location = "Unknown Rack";
        cout << "Invalid choice! Location set to Unknown Rack.\n";
    }
    book.status = true;
    return book;
}

void AddBooks(TBook books[], int& bookCount, int MaxBOOKS) {
    char addMore = 'y';
    while ((addMore == 'y' || addMore == 'Y') && bookCount < MaxBOOKS) {
        cout << "\nAdding Book " << (bookCount + 1) << ":\n";
        books[bookCount++] = AddSingleBook();
        if (bookCount < MaxBOOKS) {
            cout << "\nDo you want to add another book? (y/n): ";
            cin >> addMore;
        }
        else {
            cout << "Maximum book limit reached.\n";
        }
    }
}

void Returnbook(TBook books[], int& bookCount) {
    char ans;
    do {
        cout << "\nIssued Books:\n";
        cout << "Sr. | Category | Title | Author | Location\n";
        int issuedIndex[100];
        int j = 0;
        for (int i = 0; i < bookCount; i++) {
            if (!books[i].status) {
                cout << (j + 1) << ". " << books[i].category << " | " << books[i].name << " | "
                    << books[i].author << " | " << books[i].location << endl;
                issuedIndex[j++] = i;
            }
        }
        if (j == 0) {
            cout << "No books to return.\n";
            return;
        }

        int choice;
        cout << "Enter the serial number to return the book: ";
        do {
            cin >> choice;
            if (choice < 1 || choice > j) {
                cout << "Invalid choice. Enter again: ";
            }
        } while (choice < 1 || choice > j);

        int bookIndex = issuedIndex[choice - 1];
        books[bookIndex].status = true;
        books[bookIndex].issuedto = 0;

        int fine = 0;
        char dmg, late;

        cout << "Is the book damaged? (y/n): ";
        cin >> dmg;
        if (dmg == 'y' || dmg == 'Y') fine += 50;

        cout << "Is the book returned late? (y/n): ";
        cin >> late;
        if (late == 'y' || late == 'Y') {
            int days;
            cout << "Enter number of late days: ";
            cin >> days;
            fine += days * 5;
        }

        cout << "\n=== Return Receipt ===\n";
        cout << "Total Fine: Rs. " << fine << "\n";
        cout << "Book return completed.\n";

        cout << "Do you want to return another book? (y/n): ";
        cin >> ans;

    } while (ans == 'y' || ans == 'Y');
}

void lendbook(TBook books[], int& bookCount) {
    char ans;
    do {
        cout << "\nAvailable Books:\n";
        cout << "Sr. | Category | Title | Author | Location\n";
        int availableIndex[100];
        int j = 0;
        for (int i = 0; i < bookCount; i++) {
            if (books[i].status) {
                cout << (j + 1) << ". " << books[i].category << " | " << books[i].name << " | "
                    << books[i].author << " | " << books[i].location << endl;
                availableIndex[j++] = i;
            }
        }

        if (j == 0) {
            cout << "No available books for lending.\n";
            return;
        }

        int choice;
        cout << "Enter the serial number to lend: ";
        do {
            cin >> choice;
            if (choice < 1 || choice > j) {
                cout << "Invalid choice. Enter again: ";
            }
        } while (choice < 1 || choice > j);

        int bookIndex = availableIndex[choice - 1];
        int arid;
        cout << "Enter last 4 digits of your ARID number: ";
        cin >> arid;

        bool alreadyIssued = false;
        for (int i = 0; i < bookCount; i++) {
            if (books[i].issuedto == arid) {
                alreadyIssued = true;
                break;
            }
        }

        if (alreadyIssued) {
            cout << "You have already issued a book. Return it first.\n";
        }
        else {
            books[bookIndex].issuedto = arid;
            books[bookIndex].status = false;
            cout << "Book issued successfully!\n";
            cout << "Title: " << books[bookIndex].name << "\n";
        }

        cout << "Do you want to lend another book? (y/n): ";
        cin >> ans;

    } while (ans == 'y' || ans == 'Y');
}

void Viewbook(TBook books[], int& bookCount) {
    cout << "\nAll Available Books:\n";
    for (int i = 0, j = 1; i < bookCount; i++) {
        if (books[i].status) {
            cout << "\nBook " << j++ << ":\n";
            cout << "Category: " << books[i].category << "\n"
                << "Title: " << books[i].name << "\n"
                << "Author: " << books[i].author << "\n"
                << "Location: " << books[i].location << "\n";
        }
    }
}

int main() {
    const int MaxBOOKS = 100;
    TBook books[MaxBOOKS];
    int bookCount = 0;

    // Adding some default books
    books[bookCount++] = { "Shakespeare's Sonnets", "William Shakespeare", "English", "Rack 1", false, 9876 };
    books[bookCount++] = { "Pride and Prejudice", "Jane Austen", "English", "Rack 2", false, 5678 };
    books[bookCount++] = { "To Kill a Mockingbird", "Harper Lee", "English", "Rack 3", false, 9012 };
    books[bookCount++] = { "C++ Programming", "Bjarne Stroustrup", "Programming", "Rack 1", true };
    books[bookCount++] = { "Clean Code", "Robert C. Martin", "Development", "Rack 2", true };
    books[bookCount++] = { "Design Patterns", "Erich Gamma", "Architecture", "Rack 3", true };

    cout << "\n\t\tWELCOME TO THE FUTURE LIBRARY MANAGEMENT SYSTEM\n";

    char choice, exitChoice;
    do {
        cout << "\nWhat do you want to do?\n"
            << "L - Lend Book\n"
            << "R - Return Book\n"
            << "A - Add Book\n"
            << "V - View Books\n"
            << "Enter your choice: ";
        cin >> choice;

        switch (tolower(choice)) {
        case 'l': lendbook(books, bookCount); break;
        case 'r': Returnbook(books, bookCount); break;
        case 'a': AddBooks(books, bookCount, MaxBOOKS); break;
        case 'v': Viewbook(books, bookCount); break;
        default: cout << "Invalid option. Try again.\n";
        }

        cout << "\nDo you want to perform another operation? (y/n): ";
        cin >> exitChoice;

    } while (exitChoice == 'n' || exitChoice == 'N');

    cout << "\nThank you for using the Library Management System!\n";
    return 0;
}
