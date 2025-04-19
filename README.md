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
## Q.2 Average Age of All Users
```js
[
  {
    $group: {
      _id: null,
      averageAge: { $avg: "$age" }
    }
  }
]
```
## Q3. Top 5 Favorite Fruits
```js
[
  {
    $group: {
      _id: "$favoriteFruit",
      count: { $sum: 1 }
    }
  },
  {
    $sort: { count: -1 }
  },
  {
    $limit: 5
  }
]
```
## Q4. Gender count
```js
[
  {
    $group: {
      _id: "$gender",
      genderCount: { $sum: 1 }
    }
  }
]
```
## Q5.  Country with Most Users
```js
[
  {
    $group: {
      _id: "$company.location.country",
      userCount: { $sum: 1 }
    }
  },
  {
    $sort: { userCount: -1 }
  },
  {
    $limit: 1
  }
]
```
## Q6. Unique Eye Colors
```js
[
  {
    $group: {
      _id: "$eyeColor"
    }
  }
]
```
## Q7. Average Tags Per User
### Option A (Using $unwind):
```js
[
  { $unwind: "$tags" },
  {
    $group: {
      _id: "$_id",
      numberOfTags: { $sum: 1 }
    }
  },
  {
    $group: {
      _id: null,
      averageNumberOfTags: { $avg: "$numberOfTags" }
    }
  }
]
```
### Option B (Using $size and $addFields):
```js
[
  {
    $addFields: {
      numberOfTags: { $size: { $ifNull: ["$tags", []] } }
    }
  },
  {
    $group: {
      _id: null,
      averageNumberOfTags: { $avg: "$numberOfTags" }
    }
  }
]
```
## Q8. Users with "enim" Tag
```js
[
  {
    $match: { tags: "enim" }
  },
  {
    $count: "userWithEnimTag"
  }
]
```
## Q9. Inactive Users with "velit" Tag
```js
[
  {
    $match: {
      isActive: false,
      tags: "velit"
    }
  },
  {
    $project: {
      name: 1,
      age: 1
    }
  }
]
```
## Q10. Users with Phone Starting +1 (940)
```js
[
  {
    $match: {
      "company.phone": { $regex: /^\+1 \(940\)/ }
    }
  },
  {
    $count: "usersWithSpecialPhoneNumber"
  }
]
```
## Q11. Recently Registered Users
```js
[
  {
    $sort: { registered: -1 }
  },
  {
    $limit: 5
  },
  {
    $project: {
      name: 1,
      favoriteFruit: 1,
      registered: 1
    }
  }
]
```
## Q12. Group Users by Favorite Fruit
```js
[
  {
    $group: {
      _id: "$favoriteFruit",
      users: { $push: "$name" }
    }
  }
]
```
## Q13. "ad" as Second Tag
```js
[
  {
    $match: {
      "tags.1": "ad"
    }
  },
  {
    $count: "numberOfAdTagUsers"
  }
]
```
## Q14. Users with All Tags enim and id
```js
[
  {
    $match: {
      tags: { $all: ["enim", "id"] }
    }
  }
]
```
## Q15. Companies in USA and User Count
```js
[
  {
    $match: {
      "company.location.country": "USA"
    }
  },
  {
    $group: {
      _id: "$company.title",
      userCount: { $sum: 1 }
    }
  }
]
```
## Q16. Get Author Details using $lookup
### Option A (Using $first):
```js
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details"
    }
  },
  {
    $addFields: {
      author_details: { $first: "$author_details" }
    }
  }
]
```
### Option B (Using $arrayElementAt):
```js
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details"
    }
  },
  {
    $addFields: {
      author_details: { $arrayElementAt: ["$author_details", 0] }
    }
  }
]
```
