import os

class Library:
    def __init__(self):
        self.file_name = 'books.txt'
        self.file = open(self.file_name, 'a+')
        self.books = self.read_books()

    def __del__(self):
        self.file.close()

    def read_books(self):
        self.file.seek(0)
        lines = self.file.readlines()
        books = []
        for line in lines:
            bookinfo = line.strip().split(',')
            book = {'title': bookinfo[0], 'author': bookinfo[1], 'release_date': int(bookinfo[2]), 'pages': int(bookinfo[3])}
            books.append(book)
        return books

    def list_books(self):
        for book in self.books:
            print(f"Book: {book['title']}")
            print(f"Author: {book['author']}")
            print(f"Release Date: {book['release_date']}")
            print(f"Number of Pages: {book['pages']}")
            print()

    def add_book(self):
        booktitle = input("Enter book title: ")
        while booktitle.isdigit():
            print("Book title cannot be a numeric value.")
            booktitle = input("Enter book title: ")
        author = input("Enter book author: ")
        while True:
            try:
                date = int(input("Enter book release date (YYYY): "))
                break
            except ValueError:
                print("Release date must be a numeric value.")
        while True:
            try:
                page = int(input("Enter number of pages:(please enter number) "))
                break
            except ValueError:
                print("Number of pages must be a numeric value.")
        bookinfo = f"{booktitle},{author},{date},{page}\n"
        self.file.write(bookinfo)
        self.books.append({'title': booktitle, 'author': author, 'release_date': date, 'pages': page})
        print("Book added successfully.")

    def remove_book(self):
        print("Current book titles:")
        for book in self.books:
            print(book['title'])
        book_title = input("Enter book title to remove: ")
        while book_title.isdigit():
            print("Book title cannot be a numeric value.")
            book_title = input("Enter book title to remove: ")
        index = -1
        for i, book in enumerate(self.books):
            if book['title'] == book_title:
                index = i
                break
        if index != -1:
            self.books.pop(index)
            self.file.seek(0)
            self.file.truncate()
            for book in self.books:
                book_info = f"{book['title']},{book['author']},{book['release_date']},{book['pages']}\n"
                self.file.write(book_info)
            print("Book removed successfully.")
        else:
            print("Book not found.")

lib = Library()

while True:
    print("*** MENU*")
    print("1) List Books")
    print("2) Add Book")
    print("3) Remove Book")
    print("4) Exit")
    menu_item = int(input("Enter menu item: "))
    if menu_item == 1:
        lib.list_books()
    elif menu_item == 2:
        lib.add_book()
    elif menu_item == 3:
        lib.remove_book()
    elif menu_item == 4:
        break
    else:
        print("Invalid menu item")
