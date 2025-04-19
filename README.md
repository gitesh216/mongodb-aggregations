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
👉 Filters only active users and counts them.
## Q.2 What is the average age of all users
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
👉 Groups all users and calculates the average age.
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
👉 Groups by favorite fruit, sorts them in descending order, and limits the result to the top 5.
## Q4. Find the total number of males and females
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
👉 Groups users by gender and counts each group.
## Q5.  Which country has the highest number of registered users?
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
👉 Drills into nested fields to count users by country and returns the top one.
## Q6.  List all unique eye colors
```js
[
  {
    $group: {
      _id: "$eyeColor"
    }
  }
]
```
👉 Groups by eye color to extract unique values.
## Q7. What is the average number of tags per user?
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
👉 Two methods to compute the average number of tags — one by unwinding, and one using array size.
## Q8. Count users having "enim" as one of their tags
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
👉 Filters documents that contain "enim" in tags and counts them.
## Q9. Names and ages of inactive users who have "velit" as a tag
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
👉 Filters based on multiple fields and projects only required values.
## Q10. How many users have a phone number starting with +1 (940)?
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
👉 Uses regex to match phone numbers starting with +1 (940).
## Q11. Who has registered most recently? (Top 5)
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
👉 Sorts by registration date and returns top 5 recent users.
## Q12. Categorize users by their favorite fruits
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
👉 Groups users by fruit and lists all names in each category.
## Q13. How many users have "ad" as the second tag?
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
👉 Uses array index to match "ad" as the second tag.
## Q14. Find users who have both "enim" and "id" as their tags
```js
[
  {
    $match: {
      tags: { $all: ["enim", "id"] }
    }
  }
]
```
👉 $all ensures both values exist in the array.
## Q15. List all companies located in the USA with user count
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
👉 Filters for companies in the USA and counts users per company.
## Q16. Get Author details based on author_id
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
👉 Performs a lookup to join with the authors collection and flattens the joined array.

