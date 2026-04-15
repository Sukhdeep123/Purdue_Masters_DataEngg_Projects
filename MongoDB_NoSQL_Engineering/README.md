# MongoDB NoSQL Database Engineering

**Program:** PG Data Engineering — Purdue University (via Simplilearn)  
**Environment:** MongoDB Community Edition + MongoDB Shell (mongosh)  
**Datasets:** E-commerce product catalog, Movie database

---

## Overview

This project provides a comprehensive deep-dive into **NoSQL database engineering with MongoDB** — covering document data modelling, CRUD operations, complex aggregation pipelines, text search, and practical query optimization. The work bridges conceptual understanding of the document model with hands-on implementation across two real-world inspired datasets: an e-commerce product catalog and a movie database.

The project demonstrates when and why to choose a document database over a relational one, and how to design schemas that leverage MongoDB's flexible, denormalized document structure for performance at scale.

---

## Why MongoDB? — Design Philosophy

Unlike relational databases that enforce rigid schemas and normalize data across tables with foreign keys, MongoDB stores related data **together in documents** (JSON-like BSON structures). This trades some write redundancy for dramatically faster reads — ideal for product catalogs, user profiles, content management, and event logs where read performance dominates.

Key design decisions explored in this project:
- **Embedding vs referencing** — when to embed sub-documents vs use `$lookup` joins
- **Array fields** — storing tags, categories, and reviews as arrays within a single document
- **Schema flexibility** — handling documents with varying fields in the same collection

---

## Dataset 1 — E-Commerce Product Catalog

### Collections Designed

| Collection | Description |
|---|---|
| `products` | Product documents with name, category, price, rating, tags array, stock |
| `orders` | Order documents with embedded line items and customer reference |
| `customers` | Customer profiles with purchase history and address sub-document |

### Operations Implemented

**Collection Setup & Inserts**
- Created collections with `db.createCollection()`
- Bulk inserted product documents with `insertMany()` covering multiple categories (Electronics, Books, Clothing, Home & Kitchen)
- Inserted nested documents with array fields (`tags`, `reviews`, `variants`)

**CRUD Operations**
- `find()` with field projection and query operators (`$gt`, `$lt`, `$in`, `$regex`)
- `updateOne()` / `updateMany()` with `$set`, `$inc`, `$push` update operators
- `deleteOne()` with conditional filters
- Upsert operations combining insert + update semantics

**Aggregation Pipeline — Revenue by Category**
```javascript
db.orders.aggregate([
  { $unwind: "$lineItems" },
  { $group: { _id: "$lineItems.category", totalRevenue: { $sum: "$lineItems.subtotal" } } },
  { $sort: { totalRevenue: -1 } }
])
```
Computed total revenue grouped by product category across all orders using `$unwind` to flatten embedded arrays before grouping.

**Top Products by Rating**
```javascript
db.products.aggregate([
  { $match: { numReviews: { $gte: 10 } } },
  { $sort: { avgRating: -1 } },
  { $limit: 10 },
  { $project: { name: 1, category: 1, avgRating: 1, price: 1 } }
])
```
Filtered to products with sufficient review volume, then ranked by average rating.

---

## Dataset 2 — Movie Database

### Collection: `movies`

Documents contain: title, year, genres array, director, cast array, IMDB rating, runtime, visitor/view count.

### Queries Implemented

**Visitor/Views Analysis**
```javascript
// Most-watched movies
db.movies.find({}, { title: 1, visitors: 1 }).sort({ visitors: -1 }).limit(20)

// Average visitors by genre
db.movies.aggregate([
  { $unwind: "$genres" },
  { $group: { _id: "$genres", avgVisitors: { $avg: "$visitors" }, count: { $sum: 1 } } },
  { $sort: { avgVisitors: -1 } }
])
```

**Genre Distribution**
- Used `$unwind` on the `genres` array to explode multi-genre movies into individual genre records
- Aggregated document counts per genre to understand catalog composition
- Identified most common genre combinations using `$group` on full genres array

**Text Search Index**
```javascript
db.movies.createIndex({ title: "text", description: "text" })
db.movies.find({ $text: { $search: "space adventure" } }, { score: { $meta: "textScore" } })
   .sort({ score: { $meta: "textScore" } })
```
Built a compound text index on title and description fields, enabling full-text search with relevance scoring.

---

## Advanced Concepts Covered

### Aggregation Pipeline Stages Used
| Stage | Purpose |
|---|---|
| `$match` | Filter documents before aggregation |
| `$group` | Group and aggregate with `$sum`, `$avg`, `$count` |
| `$sort` | Order results |
| `$limit` | Top-N results |
| `$project` | Shape output documents |
| `$unwind` | Flatten array fields for per-element aggregation |
| `$lookup` | Left outer join between collections |

### Indexing Strategy
- Single field indexes on high-cardinality filter fields (`category`, `year`)
- Compound indexes for common multi-field query patterns
- Text indexes for full-text search capability
- `explain()` used to verify index utilization and compare query plans

### NoSQL vs Relational Trade-offs

| Consideration | MongoDB (Document) | Relational (SQL) |
|---|---|---|
| Schema | Flexible, per-document | Rigid, enforced |
| Joins | `$lookup` (avoid when possible) | Native JOIN, optimized |
| Read performance | High (embedded data) | Medium (requires joins) |
| Write complexity | Higher (denormalized) | Lower (normalized) |
| Best for | Catalogs, profiles, events | Transactions, reporting |

---

## Tech Stack

| Category | Tools |
|---|---|
| Database | MongoDB Community Edition |
| Query Language | MQL (MongoDB Query Language) |
| Shell | mongosh (MongoDB Shell) |
| Aggregation | MongoDB Aggregation Pipeline |
| Indexing | B-tree, Text, Compound indexes |
| Language | JavaScript (mongosh scripts) |

---

## Files

```
MongoDB_NoSQL_Engineering/
├── MongoDB_NoSQL_Engineering.ipynb    # Full notebook with queries & outputs
└── README.md
```

---

## Academic Context

**Course:** NoSQL Database Engineering & MongoDB  
**Program:** Post Graduate Program in Data Engineering  
**Institution:** Purdue University (delivered via Simplilearn)  
**Domain:** Database Design, NoSQL, Document Stores, Query Optimization
