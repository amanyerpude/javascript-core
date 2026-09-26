
---

> [!quote] Metadata  
> **Posted on:** August 13, 2023  
> **Author:** 
> **Posted in:** Interview, Javascript  
> **Tags:** #search #inmemory #class

---

# Implement an In-Memory Search Engine

Implement an in-memory search engine where multiple documents could be stored under a particular namespace, searched, and sorted by passing the `orderBy` parameter.

---

## Problem Statement

- Support multiple **namespaces** (e.g., `Movies`, `Books`).
    
- Each namespace can have multiple **documents**.
    
- Provide a **search** method that:
    
    - Accepts a `namespace`.
        
    - Accepts a `filter` function for selecting documents.
        
    - Optionally accepts an `orderBy` object to sort results by a `key` in ascending/descending order.
        

---

> [!example] Example
> 
> ```javascript
> const searchEngine = new InMemorySearch();
> 
> searchEngine.addDocuments('Movies', 
>   {name: 'Avenger', rating: 8.5, year: 2017}, 
>   {name: 'Black Adam', rating: 8.7, year: 2022}, 
>   {name: 'Jhon Wick 4', rating: 8.2, year: 2023}, 
>   {name: 'Black Panther', rating: 9.0, year: 2022}
> );
> 
> console.log(searchEngine.search(
>   'Movies', 
>   (e) => e.rating > 8.5, 
>   {key: 'rating', asc: false}
> ));
> ```
> 
> **Output:**
> 
> ```javascript
> [
>   { "name": "Black Panther", "rating": 9.0, "year": 2022 },
>   { "name": "Black Adam", "rating": 8.7, "year": 2022 }
> ]
> ```

---

## Approach

- Use a **Map** to track namespaces and their associated documents.
    
- **`registerNameSpace(name)`** → creates an empty namespace.
    
- **`addDocuments(namespace, ...docs)`** → adds one or more documents.
    
    - If namespace does not exist → create it.
        
    - If it exists → merge new documents with existing ones.
        
- **`search(namespace, filterFn, orderBy)`** → filters documents and optionally sorts them:
    
    - `filterFn` is applied to all documents.
        
    - If `orderBy` is provided, results are sorted by `orderBy.key` in ascending (`asc: true`) or descending (`asc: false`) order.
        

---

## Implementation

```javascript
class InMemorySearch {
  constructor() {
    // to track namespace and its documents
    this.entities = new Map();
  }
  
  // register a new namespace
  registerNameSpace(name) {
    this.entities.set(name, []);
  }
  
  // add documents to a namespace
  addDocuments(nameSpace, ...documents) {
    const existing = this.entities.get(nameSpace);
    
    if (!existing) {
      // create namespace if it doesn’t exist
      this.entities.set(nameSpace, [...documents]);
    } else {
      // merge with existing docs
      this.entities.set(nameSpace, [...existing, ...documents]);
    }
  }
  
  // search documents within a namespace
  search(nameSpace, filterFN, orderByFN) {
    const docs = this.entities.get(nameSpace);
    
    // filter
    const filtered = docs.filter((e) => filterFN(e));
    
    // sort if requested
    if (orderByFN) {
      const {key, asc} = orderByFN;
      return filtered.sort((a, b) => {
        return asc ? a[key] - b[key] : b[key] - a[key];
      });
    }
    
    return filtered;
  }
}
```

---

## Input

```javascript
const searchEngine = new InMemorySearch();

searchEngine.addDocuments('Movies', 
  {name: 'Avenger', rating: 8.5, year: 2017}, 
  {name: 'Black Adam', rating: 8.7, year: 2022}, 
  {name: 'Jhon Wick 4', rating: 8.2, year: 2023}, 
  {name: 'Black Panther', rating: 9.0, year: 2022}
);

console.log(searchEngine.search(
  'Movies', 
  (e) => e.rating > 8.5, 
  {key: 'rating', asc: false}
));
```

---

## Output

```javascript
[
  { "name": "Black Panther", "rating": 9.0, "year": 2022 },
  { "name": "Black Adam", "rating": 8.7, "year": 2022 }
]
```

---

> [!tip] Key Insights
> 
> - Uses **Map** to organize namespaces.
>     
> - Search supports **flexible filtering** with a callback.
>     
> - Sorting can be customized via `{ key, asc }`.
>     
> - A simple pattern to build **mini search engines** in-memory.
>     

---

### 🎥 Related Video

[Watch on YouTube](https://youtu.be/8I5RbXzhzKI)

---
