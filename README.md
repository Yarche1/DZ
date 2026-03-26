#include <iostream>
#include <fstream>
#include <cstring> 

using namespace std;

#define MAX_TITLE 100
#define MAX_BOOKS 1000
#define FILE_NAME "library.dat"

struct Book {
    char title[MAX_TITLE];
    char author[MAX_TITLE];
    int year;
};

Book library[MAX_BOOKS];
int bookCount = 0;

void addBook() {
    if (bookCount >= MAX_BOOKS) {
        cout << "Помилка: бібліотека переповнена" << endl;
        return;
    }
    
    cin.ignore(); 
    cout << "Введіть назву книги: ";
    cin.getline(library[bookCount].title, MAX_TITLE);
    
    cout << "Введіть автора: ";
    cin.getline(library[bookCount].author, MAX_TITLE);
    
    cout << "Введіть рік видання: ";
    cin >> library[bookCount].year;
    
    bookCount++;
    cout << "Книгу додано успішно" << endl;
}

void deleteBook() {
    if (bookCount == 0) {
        cout << "Бібліотека порожня." << endl;
        return;
    }
    
    int index;
    cout << "Введіть номер книги для видалення (1-" << bookCount << "): ";
    cin >> index;
    index--; 

    if (index < 0 || index >= bookCount) {
        cout << "Невірний номер" << endl;
        return;
    }

    
    for (int i = index; i < bookCount - 1; i++) {
        library[i] = library[i + 1];
    }
    bookCount--;
    cout << "Книгу видалено." << endl;
}

void searchBook() {
    cout << "1. Пошук за назвою\n2. Пошук за роком\nВибір: ";
    int mode;
    cin >> mode;

    if (mode == 1) {
        char query[MAX_TITLE];
        cout << "Введіть назву: ";
        cin.ignore();
        cin.getline(query, MAX_TITLE);
        for (int i = 0; i < bookCount; i++) {
            if (strcmp(library[i].title, query) == 0) {
                cout << "Знайдено: " << library[i].author << " - " << library[i].title << " (" << library[i].year << ")" << endl;
            }
        }
    } else {
        int year;
        cout << "Введіть рік: ";
        cin >> year;
        for (int i = 0; i < bookCount; i++) {
            if (library[i].year == year) {
                cout << "Знайдено: " << library[i].author << " - " << library[i].title << " (" << library[i].year << ")" << endl;
            }
        }
    }
}

void showAll() {
    if (bookCount == 0) {
        cout << "Бібліотека порожня." << endl;
        return;
    }
    cout << "\n--- Список книг ---" << endl;
    for (int i = 0; i < bookCount; i++) {
        cout << i + 1 << ". " << library[i].author << " - \"" << library[i].title << "\" (" << library[i].year << ")" << endl;
    }
}

void sortBooks() {
    for (int i = 0; i < bookCount - 1; i++) {
        for (int j = 0; j < bookCount - i - 1; j++) {
            if (strcmp(library[j].title, library[j + 1].title) > 0) {
                Book temp = library[j];
                library[j] = library[j + 1];
                library[j + 1] = temp;
            }
        }
    }
    cout << "Сортування за назвою виконано." << endl;
}

void saveToFile() {
    ofstream out(FILE_NAME, ios::binary);
    if (!out) return;
    
    out.write((char*)&bookCount, sizeof(int)); 
    out.write((char*)library, sizeof(Book) * bookCount); 
    out.close();
    cout << "Дані успішно збережено у " << FILE_NAME << endl;
}

void loadFromFile() {
    ifstream in(FILE_NAME, ios::binary);
    if (!in) {
        cout << "Файл не знайдено" << endl;
        return;
    }
    
    in.read((char*)&bookCount, sizeof(int));
    in.read((char*)library, sizeof(Book) * bookCount);
    in.close();
    cout << "Дані завантажено. Кількість книг: " << bookCount << endl;
}

int main() {
    int choice;
    do {
        cout << "\nБібліотека:\n";
        cout << "1. Додати книгу\n2. Видалити книгу\n3. Знайти книгу\n4. Показати всі книги\n";
        cout << "5. Сортувати книги\n6. Зберегти у файл\n7. Завантажити з файлу\n0. Вихід\n";
        cout << "Виберіть дію: ";
        cin >> choice;

        switch (choice) {
            case 1: addBook(); break;
            case 2: deleteBook(); break;
            case 3: searchBook(); break;
            case 4: showAll(); break;
            case 5: sortBooks(); break;
            case 6: saveToFile(); break;
            case 7: loadFromFile(); break;
            case 0: cout << "Завершення програми" << endl; break;
            default: cout << "Невірний вибір" << endl;
        }
    } while (choice != 0);

    return 0;
}
