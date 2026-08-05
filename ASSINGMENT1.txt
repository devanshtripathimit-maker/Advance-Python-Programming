class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_borrowed = False

    def borrow(self):
        if not self.is_borrowed:
            self.is_borrowed = True
            return True
        return False

    def return_book(self):
        self.is_borrowed = False


class Patron:
    def __init__(self, name, patron_id):
        self.name = name
        self.patron_id = patron_id
        self.borrowed_books = []

    def borrow_book(self, book):
        if book.borrow():
            self.borrowed_books.append(book)
            print(f"{self.name} borrowed '{book.title}'")
        else:
            print(f"'{book.title}' is already borrowed.")

    def return_book(self, book):
        if book in self.borrowed_books:
            book.return_book()
            self.borrowed_books.remove(book)
            print(f"{self.name} returned '{book.title}'")
        else:
            print(f"{self.name} has not borrowed '{book.title}'.")


class Library:
    def __init__(self):
        self.books = []
        self.patrons = []

    def add_book(self, book):
        self.books.append(book)

    def register_patron(self, patron):
        self.patrons.append(patron)

    def borrow_book(self, patron_id, isbn):
        patron = next((p for p in self.patrons if p.patron_id == patron_id), None)
        book = next((b for b in self.books if b.isbn == isbn), None)

        if patron and book:
            patron.borrow_book(book)
        else:
            print("Patron or Book not found.")

    def return_book(self, patron_id, isbn):
        patron = next((p for p in self.patrons if p.patron_id == patron_id), None)
        book = next((b for b in self.books if b.isbn == isbn), None)

        if patron and book:
            patron.return_book(book)
        else:
            print("Patron or Book not found.")


# Main Program
library = Library()

book1 = Book("Python Basics", "John Smith", "101")
book2 = Book("Data Structures", "Alice Brown", "102")

library.add_book(book1)
library.add_book(book2)

patron1 = Patron("Mansi", "P001")
patron2 = Patron("Rahul", "P002")

library.register_patron(patron1)
library.register_patron(patron2)

library.borrow_book("P001", "101")
library.borrow_book("P002", "101")

library.return_book("P001", "101")
library.borrow_book("P002", "101")
