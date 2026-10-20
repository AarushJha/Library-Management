# 📚 Library Management System in Python

A clean, console-based **Library Management System** built with Python and pandas, designed to run inside a Jupyter Notebook. It helps you manage a book inventory, track which books are issued or available, and maintain library records through simple interactive prompts.

---

## ✨ Project Features

| Module | Description |
|---|---|
| **Add books** | Add a new title to the system by entering its ID, name, author, and whether it is currently issued. |
| **Display all books** | View the complete catalogue as a table showing every book's ID, name, author, and issue status. |
| **Search for a book** | Look up any book instantly by its Book I.D. and see its full details. |
| **Issue a book** | Check a book out and mark it as `Issued`. Refuses if the book is already issued. |
| **Return a book** | Check a book back in and mark it as `Available`. Refuses if the book was never issued. |
| **Delete a book** | Remove an outdated or lost book from the system by its Book I.D. |


---

## 🗂️ How the Data Is Stored

Each book is identified by a unique **Book I.D.** (stored as a string, e.g. `'101'`) and carries three pieces of information:

| Field | Example |
|---|---|
| Book Name | `Concepts of Physics (Vol 1)` |
| Author | `H.C. Verma` |
| Issue Status | `Available` or `Issued` |

The initial catalogue is stored in a Python **dictionary**, where the Book I.D. is the key and the value is a list of `[name, author, status]`:

```python
books = {
    '101': ['Concepts of Physics (Vol 1)', 'H.C. Verma', 'Available'],
    '102': ['Problems in General Physics', 'I.E. Irodov', 'Issued'],
    # ...
}
```

For display, search, issue/return, and delete, the dictionary is converted into a **pandas DataFrame** with the Book I.D. as the row index:

```python
books_df = pd.DataFrame(books).T
books_df.columns = ['Book Name', 'Author', 'Issue Status']
```

---

## 💡 How to Use

### Requirements

- Python 3.x
- [pandas](https://pandas.pydata.org/)
- [Jupyter Notebook](https://jupyter.org/)

### Setup

```bash
git clone https://github.com/AarushJha/Library-Management.git
cd Library-Management
pip install pandas notebook
jupyter notebook
```

Open `Library-Management.ipynb` and run the cells **from top to bottom**, since later modules depend on the data and imports defined earlier.

### Running each module

Run the cell that defines a function once, then call it in a new cell. Prompts appear directly below the cell.

```python
add_books(books)            # Add a new book
print(books_df)             # Display all books
search_book(books_df)       # Search by Book I.D.
issue_book(books_df)        # Press 1 to issue, 2 to return
delete_book(books_df)       # Delete by Book I.D.
```

### Example session

```text
Press [1] for Issue and [2] for Return: 1
Enter Book I.D.: 101
Book issued successfully.
```

---

## 🧠 How It Works

- **Lookups use the index.** Because the Book I.D. is the DataFrame index, books are found with `df.loc[book_id]` and checked with `book_id in df.index`, with no looping required.
- **Validation before every action.** Each module first checks that the Book I.D. exists, then checks the book's current status before changing anything (for example, an already-issued book cannot be issued again).
- **In-place updates.** Issue, return, and delete modify the DataFrame directly, so changes are visible immediately in `books_df`.

---

## ⚠️ Known Limitations

- `books_df` is built from the `books` dictionary, and `add_books` currently updates only the dictionary. After adding a book, rebuild the DataFrame to see it:
  ```python
  books_df = pd.DataFrame(books).T
  books_df.columns = ['Book Name', 'Author', 'Issue Status']
  ```
  Note that rebuilding discards any issue, return, or delete changes made on the old DataFrame.
- Data is held in memory only, so everything resets when the notebook kernel restarts.
- Menu input is not yet fully guarded against non-numeric entries.

---

## 🛣️ Roadmap

- [ ] Make the DataFrame the single source of truth (have `add_books` append to it directly)
- [ ] Add a main menu loop with an **Exit** option
- [ ] Save and load the catalogue from a CSV file so data persists between sessions
- [ ] Validate user input (non-numeric menu choices, duplicate Book I.D.s)
- [ ] Prevent deleting a book that is currently issued

---

## 🛠️ Technologies Used

- **Python 3** (dictionaries, functions, conditionals)
- **pandas** (DataFrame storage, indexing with `.loc`, `drop`)
- **Jupyter Notebook** (interactive execution)

