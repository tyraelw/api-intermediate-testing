# API Intermediate Testing

An intermediate-level API testing collection demonstrating environment variables, variable chaining, pre-request scripts, and complete CRUD operations using Postman and the JSONPlaceholder API.

## 📋 Project Overview

This project showcases intermediate API testing patterns including all HTTP methods (GET, POST, PUT, PATCH, DELETE), dynamic data generation, variable management across requests, and understanding of fake vs. real API behavior. It demonstrates a complete CRUD (Create, Read, Update, Delete) workflow with connected requests.

## 🎯 Skills Demonstrated

This collection demonstrates proficiency in:

- **Implementing environment variables** for reusable configuration
- **Chaining variables between requests** to create connected workflows
- **Writing pre-request scripts** for dynamic data generation
- **Using all HTTP methods** (GET, POST, PUT, PATCH, DELETE)
- **Understanding PUT vs PATCH** differences in update operations
- **Converting data types** (parseInt) for proper comparisons
- **Generating dynamic test data** using timestamps
- **Working with complete CRUD cycles** in API testing
- **Handling API response data** and saving to variables
- **Validating complex workflows** across multiple requests
- **Understanding fake API limitations** vs. real API behavior

## 📊 Test Flow

### Connected Request Chain

```
1. GET All Posts
   ↓ Saves first post ID
   
2. GET Single Post
   ↓ Uses saved post ID
   
3. POST Create Post
   ↓ Generates dynamic title → Saves new post ID
   
4. GET Verify Created
   ↓ Uses new post ID
   
5. PUT Full Update
   ↓ Updates all fields
   
6. PATCH Partial Update
   ↓ Updates only title
   
7. DELETE Post
   ↓ Removes post
   
8. GET Verify Deletion
   └ Confirms deletion (or explains fake API behavior)
```

## 🛠️ Technologies Used

- **Postman** - API testing platform
- **JSONPlaceholder** - Free fake REST API for testing
- **JavaScript** - Test and pre-request scripting
- **Environment Variables** - Dynamic configuration management
- **JSON** - Data format

## 📝 Request Details

| # | Request | Method | Purpose | Key Features |
|---|---------|--------|---------|--------------|
| 1 | GET - All Posts | GET | Get all posts | Saves first ID to variable |
| 2 | GET - Single Post | GET | Get specific post | Uses variable from #1 |
| 3 | POST - Create Post | POST | Create new post | Pre-request script, dynamic title |
| 4 | GET - Verify Created | GET | Verify post exists | Uses variable from #3 |
| 5 | PUT - Full Update | PUT | Replace entire post | All fields required |
| 6 | PATCH - Partial Update | PATCH | Update one field | Only title sent |
| 7 | DELETE - Remove Post | DELETE | Delete post | Completes CRUD cycle |
| 8 | GET - Verify Deletion | GET | Confirm deletion | Fake API behavior notes |

**Total: 8 requests with ~30 automated tests**

## 🔑 Key Concepts Demonstrated

### 1. Environment Variables

```javascript
// Set variable
pm.environment.set("postId", firstPost.id);

// Get variable
var savedId = pm.environment.get("postId");

// Use in URL
{{baseUrl}}/posts/{{postId}}
```

### 2. Variable Chaining

Each request uses data from previous requests, creating a connected workflow:

```
Request 1: Saves postId = 1
Request 2: Uses {{postId}} in URL
Request 3: Saves newPostId = 101
Requests 4-8: Use {{newPostId}} throughout
```

### 3. Pre-request Scripts

```javascript
// Generate unique title before request
var timestamp = Date.now();
var randomTitle = "Test Post " + timestamp;
pm.environment.set("randomTitle", randomTitle);
```

### 4. Type Conversion

```javascript
// Environment variables are strings, convert for comparison
var savedId = parseInt(pm.environment.get("postId"));
pm.expect(jsonData.id).to.equal(savedId);
```

### 5. PUT vs PATCH

**PUT - Full Replacement:**
```json
{
  "id": 1,
  "title": "New Title",
  "body": "New Body",
  "userId": 1
}
```

**PATCH - Partial Update:**
```json
{
  "title": "Only Title Changed"
}
```

## 🚀 How to Run

### Prerequisites
- Postman installed ([Download here](https://www.postman.com/downloads/))

### Import Steps

1. **Import the Collection**
   - Download `api-intermediate-testing.postman_collection.json`
   - In Postman, click **Import**
   - Select the collection file
   - Collection appears in Collections sidebar

2. **Import the Environment**
   - Download `jsonplaceholder-test.postman_environment.json`
   - In Postman, click **Import**
   - Select the environment file
   - Environment appears in Environments section

3. **Select the Environment**
   - Click the environment dropdown (top right)
   - Select **JSONPlaceholder-Test**
   - ⚠️ **CRITICAL:** Must select environment or requests will fail

### Running the Collection

**⚠️ IMPORTANT:** Requests must run in order due to variable dependencies

**Option 1: Collection Runner (Recommended)**
1. Right-click on **API Intermediate Testing** collection
2. Select **Run collection**
3. Ensure requests are in order (1-8)
4. Click **Run API Intermediate Testing**
5. Watch variables pass between requests

**Option 2: Manual (Educational)**
- Run each request individually in order (1 → 8)
- Watch Console tab to see variables being saved
- See how each request uses data from previous ones

### Expected Results

```
✅ All 8 requests pass
✅ Variables successfully chain between requests
✅ Console shows:
   - "Saved postId: 1"
   - "Generated title: Test Post [timestamp]"
   - "Set newPostId to 1 for subsequent requests"
   - "CRUD Operations Completed"
```

## 🎓 Skills Demonstrated

### Advanced Postman Features
- Environment variable management
- Pre-request script execution
- Variable chaining across requests
- Dynamic data generation with Date.now()
- Console logging for debugging

### API Testing Best Practices
- Complete CRUD cycle implementation
- Understanding HTTP method semantics
- Proper status code validation (200, 201, 204)
- Response data structure validation
- Connected workflow testing

### JavaScript Proficiency
- Variable manipulation and storage
- Type conversion (parseInt)
- String concatenation
- Object property access
- Array manipulation
- Conditional logic

### Quality Assurance Mindset
- Test organization and sequencing
- Clear test documentation
- Understanding test vs. production environments
- Handling edge cases (fake API behavior)
- Comprehensive validation patterns

## 📋 Environment Variables

| Variable | Purpose | Set By | Used By |
|----------|---------|--------|---------|
| `baseUrl` | API base URL | Manual setup | All requests |
| `postId` | First post ID | Request 1 | Request 2 |
| `newPostId` | Created post ID | Request 3 | Requests 4-8 |
| `randomTitle` | Dynamic title | Request 3 (pre-request) | Request 3 |

## 💡 Understanding JSONPlaceholder

**What is JSONPlaceholder?**
- Free fake REST API for testing and prototyping
- Simulates CRUD operations without persisting data
- Perfect for demonstrating API testing concepts

**Key Limitations:**
- ✅ POST returns success (201) but doesn't actually save
- ✅ PUT/PATCH return success but don't persist changes
- ✅ DELETE returns success but doesn't remove data
- ✅ Subsequent GETs still return original data

**Why This Matters:**
This demonstrates understanding of the difference between test/mock APIs and production systems. In a real scenario:
- Request 3 would create a real post with a unique ID
- Requests 5-6 would permanently modify the post
- Request 7 would permanently delete the post
- Request 8 would return 404 (Not Found)

The test logic is production-ready; only the API backend is simulated.

## 🔗 Related Projects

- [Basic API Testing](../basic-api-testing) - Core API testing patterns
- [Trello API Testing](../trello-api-testing) - Real API with authentication and Newman
- [Simple Grocery Store API](../simple-grocery-store-api-testing) - E-commerce flow validation

## 📌 Portfolio Context

This project is part of a structured portfolio demonstrating comprehensive API testing expertise across different complexity levels. While my advanced projects like [Trello API Testing](../trello-api-testing) showcase production-ready implementations with authentication, Newman CLI, and CI/CD, this collection focuses on intermediate patterns that bridge fundamental and advanced concepts.

**Focus Area:** Intermediate API testing patterns
- Complete CRUD operations
- Environment variable management
- Request chaining and data flow
- Pre-request script automation
- Dynamic test data generation

**Related Skill Levels:**
- **Fundamental:** [Basic API Testing](../basic-api-testing) - Core GET/POST patterns
- **Advanced:** [Trello API Testing](../trello-api-testing) - Authentication, Newman, CI/CD, dynamic workflows

## 📝 Notes

- **API Used**: JSONPlaceholder - Free fake REST API for testing
- **Purpose**: Demonstrates intermediate API testing patterns and request chaining
- **Test Data**: Dynamically generated using timestamps
- **No Authentication**: Focuses on CRUD operations and variable management

## 📧 Contact

For questions about this project or intermediate API testing patterns, feel free to reach out via [tyrael78w@gmail.com](mailto:tyrael78w@gmail.com)

---

**Part of QA Testing Portfolio** | **November 2025**
