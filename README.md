# 📘 MongoDB Aggregation Notes

This repo contains a collection of useful MongoDB aggregation queries with syntax-correct examples and explanations. Perfect for quick reference and learning!

---

## 📑 Table of Contents

### 🔹 Basic Operators
- [Q1. Count Active Users (`$match`, `$count`)](#q1-count-active-users)
- [Q2. Average Age of All Users (`$group`, `$avg`)](#q2-average-age-of-all-users)
- [Q3. Top 5 Favorite Fruits (`$group`, `$sort`, `$limit`)](#q3-top-5-favorite-fruits)
- [Q4. Gender Count (`$group`)](#q4-gender-count)
- [Q5. Country with Most Users (`$group`, `$sort`, `$limit`)](#q5-country-with-most-users)
- [Q6. Unique Eye Colors (`$group`)](#q6-unique-eye-colors)

### 🔹 Arrays and Advanced
- [Q7. Average Tags Per User (`$unwind`, `$group`, `$addFields`, `$size`)](#q7-average-tags-per-user)
- [Q8. Users with "enim" Tag (`$match`, `$count`)](#q8-users-with-enim-tag)
- [Q9. Inactive Users with "velit" Tag (`$match`, `$project`)](#q9-inactive-users-with-velit-tag)
- [Q10. Users with Phone Starting `+1 (940)` (`$match`, Regex)](#q10-users-with-phone-starting-1-940)
- [Q11. Recently Registered Users (`$sort`, `$limit`, `$project`)](#q11-recently-registered-users)
- [Q12. Group Users by Favorite Fruit (`$group`, `$push`)](#q12-group-users-by-favorite-fruit)
- [Q13. "ad" as Second Tag (`$match`, Array Indexing)](#q13-ad-as-second-tag)
- [Q14. Users with All Tags `enim` and `id` (`$match`, `$all`)](#q14-users-with-all-tags-enim-and-id)
- [Q15. Companies in USA and User Count (`$match`, `$group`)](#q15-companies-in-usa-and-user-count)

### 🔹 Joins and Lookups
- [Q16. Get Author Details using `$lookup`](#q16-get-author-details-using-lookup)

---

## Q1. Count Active Users
```js
[
  {
    $match: { isActive: true }
  },
  {
    $count: "activeUsers"
  }
]
```
## Q1. Count Active Users
[
  {
    $group: {
      _id: null,
      averageAge: { $avg: "$age" }
    }
  }
]
