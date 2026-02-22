# 📚 Digital Library Management System

A feature-rich, terminal-based **Digital Library Management System** built with **Python** and **MySQL**. It supports dual-role access (Admin & User), full book and user CRUD operations, book issuing/returning with fine calculation, personal note-taking, live news fetching, and Wikipedia article lookup — all from an interactive command-line interface.

---

## 🗂️ Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Database Schema](#-database-schema)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Running the Application](#-running-the-application)
- [Application Flow](#-application-flow)
- [Return Policy & Fine Calculation](#-return-policy--fine-calculation)
- [External APIs](#-external-apis)
- [Screenshots](#-screenshots)

---

## ✨ Features

### 🔐 Authentication
- Separate login flows for **Admin** and **User** roles
- Password-based authentication with session tracking
- Admin can switch to User panel and vice versa

### 📖 Book Management *(Admin only)*
- **Add** a new book (Book ID, Name, Publication Year, Author)
- **Delete** an existing book
- **Update** book details (ID, Name, Publication Year, Author)
- **Display** all books with full details
- **Search** books by Book ID or keyword

### 👥 User Management *(Admin only)*
- **Add** a new library member (User ID, Name, Phone, Email, Password)
- **Delete** an existing user
- **Update** user details (ID, Name, Phone Number, Email, Password)
- **Display** all enrolled users
- **Search** users by User ID or keyword

### 📤 Book Issuing & Returning *(Admin only)*
- **Issue** a book to a specific user (records issue date & time)
- **Return** a book and automatically track return date & time
- **Fine calculation** for overdue returns (₹5/day after 14-day grace period)
- Full history maintained in `issuedBooksDetails` table

### 📝 Personal Notes *(User)*
- **Add** a personal note (Note Number, Title, Description)
- **Delete** a note
- **Update** a note (Number, Title, Description)
- **Display** all notes with timestamps
- **Search** notes by Note Number or keyword

### 📰 Live News Feed *(User)*
- Fetches top headlines via the **NewsAPI**
- Filter by **country code** and **category** (business, entertainment, general, health, science, sports, technology)
- Configurable number of articles

### 🌐 Wikipedia Articles *(User)*
- Search any topic and retrieve a **Wikipedia article summary**
- Configurable article summary length
- Displays article title, full URL, and formatted summary

### ℹ️ About Library
- View library establishment year, librarian name, total books, and total users

### 🔁 Admin Transfer
- The current admin can transfer admin privileges to any other registered user (with password verification)

---

## 🛠️ Tech Stack

| Component        | Technology                |
|------------------|---------------------------|
| Language         | Python 3.x                |
| Database         | MySQL                     |
| DB Connector     | `mysql-connector-python`  |
| ASCII Art Banner | `pyfiglet`                |
| News API Client  | `requests`                |
| Wikipedia Client | `wikipedia-api`           |
| Interface        | CLI (Terminal)            |

---

## 📁 Project Structure

```
Digital Library/
│
├── main.py                          # Main application entry point (all logic)
├── NEWS_API_KEY.txt                 # NewsAPI key reference file
├── README.md                        # Project documentation
│
├── project files/
│   ├── Digital Library.pdf          # Project report / documentation
│   └── editable files/              # Source/editable project assets
│
├── application screenshots/         # Screenshots of the running application
├── database screenshots/            # Screenshots of the MySQL database
└── errors & exceptions screenshots/ # Screenshots of error handling
```

---

## 🗄️ Database Schema

The application uses a MySQL database named **`library`** with the following tables:

### `books`
| Column           | Type        | Description                          |
|------------------|-------------|--------------------------------------|
| `bookId`         | INT (PK)    | Unique book identifier               |
| `bookName`       | VARCHAR     | Title of the book                    |
| `publicationYear`| INT         | Year the book was published          |
| `author`         | VARCHAR     | Author's name                        |
| `issueStatus`    | VARCHAR     | `'issued'` or `'not issued'`         |
| `issueDate`      | DATE        | Date the book was issued             |
| `issueTime`      | TIME        | Time the book was issued             |
| `returnDate`     | DATE        | Date the book was returned           |
| `returnTime`     | TIME        | Time the book was returned           |
| `issuedUserId`   | INT (FK)    | User ID to whom the book is issued   |

### `users`
| Column        | Type        | Description                          |
|---------------|-------------|--------------------------------------|
| `userId`      | INT (PK)    | Unique user identifier               |
| `userName`    | VARCHAR     | Full name of the user                |
| `phoneNumber` | VARCHAR     | User's phone number                  |
| `emailId`     | VARCHAR     | User's email address                 |
| `password`    | VARCHAR     | User's login password                |
| `adminStatus` | VARCHAR     | `'admin'` or `'not admin'`           |

### `notes`
| Column            | Type     | Description                      |
|-------------------|----------|----------------------------------|
| `userId`          | INT (FK) | Owner of the note                |
| `noteNumber`      | INT      | User-defined note number         |
| `noteTitle`       | VARCHAR  | Title of the note                |
| `noteDescription` | TEXT     | Body/content of the note         |
| `updateDate`      | DATE     | Last updated date                |
| `updateTime`      | TIME     | Last updated time                |

### `issuedBooksDetails`
| Column       | Type     | Description                           |
|--------------|----------|---------------------------------------|
| `userId`     | INT (FK) | User who issued the book              |
| `bookId`     | INT (FK) | ID of the issued book                 |
| `bookName`   | VARCHAR  | Name of the issued book               |
| `issueDate`  | DATE     | Date the book was issued              |
| `issueTime`  | TIME     | Time the book was issued              |
| `returnDate` | DATE     | Date the book was returned            |
| `returnTime` | TIME     | Time the book was returned            |
| `fineInRs`   | INT      | Fine charged in Indian Rupees (₹)     |

---

## ✅ Prerequisites

Make sure the following are installed on your system:

- **Python 3.7+** — [Download Python](https://www.python.org/downloads/)
- **MySQL Server** — [Download MySQL](https://dev.mysql.com/downloads/mysql/)
- **pip** (Python package manager)

---

## 🚀 Installation

1. **Clone or download the repository:**
   ```bash
   git clone <repository-url>
   cd "Digital Library"
   ```

2. **Install the required Python packages:**
   ```bash
   pip install mysql-connector-python pyfiglet requests wikipedia-api
   ```

3. **Set up the MySQL database:**
   - Open your MySQL client (MySQL Workbench, CLI, etc.)
   - Create the database and tables:
     ```sql
     CREATE DATABASE library;
     USE library;

     CREATE TABLE books (
         bookId INT PRIMARY KEY,
         bookName VARCHAR(255),
         publicationYear INT,
         genre VARCHAR(100),
         language VARCHAR(50),
         edition VARCHAR(50),
         copies INT,
         author VARCHAR(255),
         issueStatus VARCHAR(20) DEFAULT 'not issued',
         issueDate DATE,
         issueTime TIME,
         returnDate DATE,
         returnTime TIME,
         issuedUserId INT
     );

     CREATE TABLE users (
         userId INT PRIMARY KEY,
         userName VARCHAR(255),
         phoneNumber VARCHAR(20),
         emailId VARCHAR(255),
         password VARCHAR(255),
         adminStatus VARCHAR(20) DEFAULT 'not admin'
     );

     CREATE TABLE notes (
         userId INT,
         noteNumber INT,
         noteTitle VARCHAR(255),
         noteDescription TEXT,
         updateDate DATE,
         updateTime TIME
     );

     CREATE TABLE issuedBooksDetails (
         userId INT,
         bookId INT,
         bookName VARCHAR(255),
         issueDate DATE,
         issueTime TIME,
         returnDate DATE,
         returnTime TIME,
         fineInRs INT DEFAULT 0
     );
     ```
   - Insert the first admin user:
     ```sql
     INSERT INTO users (userId, userName, phoneNumber, emailId, password, adminStatus)
     VALUES (1, 'YourName', '9999999999', 'admin@example.com', 'yourpassword', 'admin');
     ```

---

## ⚙️ Configuration

Open `main.py` and update the database connection credentials at the top of the file:

```python
db = mysql.connector.connect(
    host="localhost",
    user="YOUR_MYSQL_USERNAME",      # e.g., "root"
    password="YOUR_MYSQL_PASSWORD",  # e.g., "password123"
    database="library",
)
```

> **Note:** The NewsAPI key is already embedded in the `news()` function. To use your own key, replace the value of `API_KEY` in the `news()` function with your key from [newsapi.org](https://newsapi.org/).

---

## ▶️ Running the Application

```bash
python main.py
```

The application will launch with an ASCII art welcome banner and present the **Home Menu**.

---

## 🗺️ Application Flow

```
Home
├── 1. Admin (requires authentication)
│   ├── Login into User Panel
│   ├── Modify User
│   │   ├── Add User
│   │   ├── Delete User
│   │   └── Update User Details
│   ├── Display Users
│   ├── Search Users (by ID / keyword)
│   ├── Modify Book
│   │   ├── Add Book
│   │   ├── Delete Book
│   │   └── Update Book Details
│   ├── Issue Book
│   ├── Return Book
│   └── Change Admin
│
├── 2. User (requires authentication)
│   ├── About Library
│   ├── News
│   ├── Wikipedia Articles
│   ├── Display Books
│   ├── Search Books (by ID / keyword)
│   ├── Issued Books Details
│   └── Notes
│       ├── Modify Note (Add / Delete / Update)
│       ├── Display Notes
│       └── Search Notes (by Number / keyword)
│
└── 3. Exit
```

---

## 📋 Return Policy & Fine Calculation

- Books must be returned within **14 days (2 weeks)** of issue.
- For every extra day beyond the 14-day period, a fine of **₹5 per day** is charged.
- The fine is automatically calculated and recorded in the `issuedBooksDetails` table upon return.

> **Example:** If a book is kept for 20 days, the extra days = 20 - 14 = **6 days**, and the fine = 6 × ₹5 = **₹30**.

---

## 🌐 External APIs

| API            | Provider    | Purpose                          | Documentation                      |
|----------------|-------------|----------------------------------|------------------------------------|
| **NewsAPI**    | newsapi.org | Fetch top news headlines         | [newsapi.org/docs](https://newsapi.org/docs) |
| **Wikipedia**  | wikipedia-api | Fetch Wikipedia article summaries | [PyPI: wikipedia-api](https://pypi.org/project/Wikipedia-API/) |

---

## 📸 Screenshots

Screenshots of the application in action are available in the following directories:

- **`application screenshots/`** — Shows various menus and features of the running application
- **`database screenshots/`** — Shows the MySQL database tables and records
- **`errors & exceptions screenshots/`** — Shows how the application handles invalid inputs and errors

---

## 📄 License

This project is for educational purposes. Feel free to use and modify it for learning and personal projects.

---

*Built with ❤️ using Python & MySQL*
