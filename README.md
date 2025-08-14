use plp_bookstore
db.createCollection("books")
db.books.insertMany([
  {
    title: "A Game of Thrones",
    author: "George R. R. Martin",
    genre: "Fantasy",
    published_year: 1996,
    price: 20.99,
    in_stock: true,
    pages: 694,
    publisher: "Bantam Books"
  },
  {
    title: "A Clash of Kings",
    author: "George R. R. Martin",
    genre: "Fantasy",
    published_year: 1998,
    price: 21.99,
    in_stock: true,
    pages: 768,
    publisher: "Bantam Books"
  },
  {
    title: "A Storm of Swords",
    author: "George R. R. Martin",
    genre: "Fantasy",
    published_year: 2000,
    price: 22.99,
    in_stock: false,
    pages: 973,
    publisher: "Bantam Books"
  },
  {
    title: "A Feast for Crows",
    author: "George R. R. Martin",
    genre: "Fantasy",
    published_year: 2005,
    price: 19.99,
    in_stock: true,
    pages: 753,
    publisher: "Bantam Books"
  },
  {
    title: "A Dance with Dragons",
    author: "George R. R. Martin",
    genre: "Fantasy",
    published_year: 2011,
    price: 24.99,
    in_stock: true,
    pages: 1016,
    publisher: "Bantam Books"
  },
  // Add at least 5 more books of your choice
]);
mongosh
load("insert_books.js")
// 1. Find all books in a specific genre
db.books.find({ genre: "Fantasy" });

// 2. Find books published after 2010
db.books.find({ published_year: { $gt: 2010 } });

// 3. Find books by a specific author
db.books.find({ author: "George R. R. Martin" });

// 4. Update the price of a specific book
db.books.updateOne({ title: "A Game of Thrones" }, { $set: { price: 18.99 } });

// 5. Delete a book by its title
db.books.deleteOne({ title: "A Clash of Kings" });
// Books in stock and published after 2010
db.books.find({ in_stock: true, published_year: { $gt: 2010 } });

// Projection (only title, author, price)
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 });

// Sorting by price
db.books.find().sort({ price: 1 }); // ascending
db.books.find().sort({ price: -1 }); // descending

// Pagination (page 1, 5 per page)
db.books.find().skip(0).limit(5);
// Page 2
db.books.find().skip(5).limit(5);
// Average price by genre
db.books.aggregate([
  { $group: { _id: "$genre", avgPrice: { $avg: "$price" } } }
]);

// Author with most books
db.books.aggregate([
  { $group: { _id: "$author", bookCount: { $sum: 1 } } },
  { $sort: { bookCount: -1 } },
  { $limit: 1 }
]);

// Group by publication decade
db.books.aggregate([
  { $group: {
      _id: { $subtract: [ { $divide: ["$published_year", 10] }, { $mod: [ { $divide: ["$published_year", 10] }, 1 ] } ] },
      count: { $sum: 1 }
  } }
]);
// Index on title
db.books.createIndex({ title: 1 });

// Compound index on author + published_year
db.books.createIndex({ author: 1, published_year: 1 });

// Check performance
db.books.find({ title: "A Game of Thrones" }).explain("executionStats");
git clone <your-repo-url>
cd <repo-folder>
git add .
git commit -m "MongoDB Week 1 project with Game of Thrones books"
git push
