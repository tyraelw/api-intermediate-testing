# API Intermediate Testing

An intermediate-level API testing collection demonstrating environment variables, variable chaining, pre-request scripts, and complete CRUD operations using Postman and the JSONPlaceholder API.

## 📋 Project Overview

This project showcases intermediate API testing skills including all HTTP methods (GET, POST, PUT, PATCH, DELETE), dynamic data generation, variable management across requests, and understanding of fake vs. real API behavior. It demonstrates a complete CRUD (Create, Read, Update, Delete) workflow with connected requests.

## 🎯 What I Learned

This collection demonstrates my ability to:

- **Implement environment variables** for reusable configuration
- **Chain variables between requests** to create connected workflows
- **Write pre-request scripts** for dynamic data generation
- **Use all HTTP methods** (GET, POST, PUT, PATCH, DELETE)
- **Understand PUT vs PATCH** differences in update operations
- **Convert data types** (parseInt) for proper comparisons
- **Generate dynamic test data** using timestamps
- **Work with complete CRUD cycles** in API testing
- **Handle API response data** and save to variables
- **Validate complex workflows** across multiple requests
- **Understand fake API limitations** vs. real API behavior

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
- Perfect for learning and demonstrating API testing concepts

**Key Limitations:**
- ✅ POST returns success (201) but doesn't actually save
- ✅ PUT/PATCH return success but don't persist changes
- ✅ DELETE returns success but doesn't remove data
- ✅ Subsequent GETs still return original data

**Why This Matters:**
This project demonstrates that I understand the difference between test/mock APIs and production systems. In a real scenario:
- Request 3 would create a real post with a unique ID
- Requests 5-6 would permanently modify the post
- Request 7 would permanently delete the post
- Request 8 would return 404 (Not Found)

The test logic is production-ready; only the API backend is simulated.

## 📈 Progression from Basic to Intermediate

### Basic API Testing (Previous Project)
- ✅ Simple GET and POST requests
- ✅ Independent requests
- ✅ Basic validations
- ✅ No variables
- ✅ Static data

### Intermediate API Testing (This Project)
- ✅ All HTTP methods (GET, POST, PUT, PATCH, DELETE)
- ✅ Connected request workflow
- ✅ Environment variables
- ✅ Variable chaining
- ✅ Pre-request scripts
- ✅ Dynamic data generation
- ✅ Complete CRUD cycle
- ✅ Advanced validation patterns

## 🔗 Related Projects

- [Basic API Testing](../basic-api-testing) - Foundation API testing skills
- [Trello API Advanced Testing](../trello-api-testing) - Real API with authentication
- [Cypress Intermediate Tests](../cypress-intermediate-tests) - UI automation with data-driven patterns

## 📧 Questions?

Feel free to reach out if you have questions about this project or want to discuss API testing patterns!

---

**Created as part of my QA Testing portfolio** | **Date: November 2025**
