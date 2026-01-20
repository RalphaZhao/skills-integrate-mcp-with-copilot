# Dev RAG Agent - Usage Examples

This document provides practical examples of how to interact with the Dev RAG Agent.

## Quick Start

To use the Dev RAG Agent in GitHub Copilot Chat:

1. Ensure the agent is configured in `.github/agents/dev-rag-agent.yml`
2. Open GitHub Copilot Chat in your editor
3. Reference the agent using `@dev-rag-agent` or select it from the agent picker

## Example Interactions

### 1. Understanding Existing Code

**Prompt:**
```
@dev-rag-agent Explain how the activity signup process works in this application
```

**What the agent does:**
- Retrieves the `signup_for_activity` function from `src/app.py`
- Analyzes the activities data structure
- Explains the validation logic and error handling
- References related endpoints and data models

**Expected Response:**
The agent will provide a detailed explanation including:
- How activities are stored in the in-memory dictionary
- The validation checks (activity exists, not already signed up)
- How participants are added to the activity list
- Error cases and HTTP exceptions

---

### 2. Adding a New Feature

**Prompt:**
```
@dev-rag-agent Create a new GET endpoint that returns activities filtered by a maximum participant count
```

**What the agent does:**
- Searches for existing GET endpoints to match the pattern
- Reviews the current `/activities` endpoint structure
- Creates a new endpoint following FastAPI conventions
- Maintains consistency with error handling and response format

**Expected Code Generation:**
```python
@app.get("/activities/by-max-participants/{max_count}")
def get_activities_by_max_participants(max_count: int):
    """Get activities with a maximum participant limit"""
    if max_count < 1:
        raise HTTPException(
            status_code=400,
            detail="Max count must be at least 1"
        )
    
    filtered_activities = {
        name: details 
        for name, details in activities.items() 
        if details["max_participants"] <= max_count
    }
    
    return filtered_activities
```

---

### 3. Debugging Issues

**Prompt:**
```
@dev-rag-agent What could cause the signup endpoint to return a 400 error?
```

**What the agent does:**
- Locates the `signup_for_activity` function
- Identifies all paths that raise HTTPException with status_code=400
- Explains each error condition

**Expected Response:**
The agent will explain that 400 errors occur when:
1. Student is already signed up for the activity
2. (If you add validation) Invalid email format
3. (If you add validation) Activity is at max capacity

---

### 4. Code Improvement

**Prompt:**
```
@dev-rag-agent Suggest improvements to the activity signup validation
```

**What the agent does:**
- Reviews current validation in `signup_for_activity`
- Searches for similar patterns in the codebase
- Suggests enhancements based on best practices

**Expected Suggestions:**
- Add email format validation
- Check if activity has reached max_participants
- Add input sanitization
- Implement better error messages
- Consider adding activity status (active/inactive)

---

### 5. Documentation Generation

**Prompt:**
```
@dev-rag-agent Add comprehensive docstrings to all functions in src/app.py
```

**What the agent does:**
- Analyzes each function's parameters and return values
- Generates Google-style or NumPy-style docstrings
- Includes descriptions of exceptions raised

**Expected Output:**
```python
@app.post("/activities/{activity_name}/signup")
def signup_for_activity(activity_name: str, email: str):
    """
    Sign up a student for an extracurricular activity.
    
    Args:
        activity_name (str): The name of the activity to sign up for
        email (str): The student's email address
    
    Returns:
        dict: A success message with the student email and activity name
    
    Raises:
        HTTPException: 
            - 404 if activity not found
            - 400 if student is already signed up
    
    Example:
        >>> signup_for_activity("Chess Club", "student@mergington.edu")
        {"message": "Signed up student@mergington.edu for Chess Club"}
    """
    # ... existing implementation
```

---

### 6. Finding Related Code

**Prompt:**
```
@dev-rag-agent Show me all endpoints that modify the activities data
```

**What the agent does:**
- Searches through `src/app.py` for functions that modify `activities`
- Identifies POST, PUT, DELETE, and PATCH endpoints
- Shows the relevant code sections

**Expected Response:**
The agent will identify:
- `signup_for_activity` (POST) - adds to participants list
- `unregister_from_activity` (DELETE) - removes from participants list
- Any other modifying endpoints

---

### 7. Testing Suggestions

**Prompt:**
```
@dev-rag-agent What test cases should I write for the signup endpoint?
```

**What the agent does:**
- Analyzes the `signup_for_activity` function
- Identifies all code paths and edge cases
- Suggests comprehensive test scenarios

**Expected Suggestions:**
1. **Success case**: Valid activity and new student email
2. **404 case**: Non-existent activity name
3. **400 case**: Student already signed up
4. **Edge cases**: 
   - Empty email string
   - Very long email
   - Special characters in activity name
   - Activity at max capacity

---

### 8. Refactoring Assistance

**Prompt:**
```
@dev-rag-agent Should the activities data be moved to a separate module?
```

**What the agent does:**
- Reviews the current structure in `src/app.py`
- Considers the application size and complexity
- Provides architectural recommendations

**Expected Response:**
The agent will provide a balanced analysis:
- **Current state**: In-memory dictionary in app.py (simple, good for small apps)
- **Pros of keeping it**: Simplicity, no overhead for small dataset
- **Pros of separating**: Better organization, easier to switch to database later
- **Recommendation**: Based on project size (currently fine as-is for this small app)

---

## Tips for Effective Agent Usage

### Be Specific
❌ "Make this better"
✅ "Add email validation to the signup endpoint"

### Provide Context
❌ "Why doesn't this work?"
✅ "Why does the signup endpoint return 400 when I try to register the same email twice?"

### Iterate
1. Start with small, focused requests
2. Review the agent's response
3. Ask follow-up questions or request modifications
4. Build up to more complex tasks

### Reference Files
❌ "How does routing work?"
✅ "How do the routes in src/app.py handle URL parameters?"

## Advanced Usage

### Combining with MCP Tools

The Dev RAG Agent works with GitHub MCP server for enhanced capabilities:

```
@dev-rag-agent Using the GitHub MCP server, find similar activity management 
projects and suggest improvements based on their implementations
```

### Multi-step Tasks

```
@dev-rag-agent 
1. Analyze the current data model for activities
2. Suggest a database schema for PostgreSQL
3. Create a migration plan to move from in-memory to database storage
```

### Code Review

```
@dev-rag-agent Review the changes in the last commit and check for:
- Consistency with existing code style
- Potential bugs or edge cases
- Missing error handling
- Documentation gaps
```

## Integration with Development Workflow

### During Development
- Use for code completion and generation
- Get explanations of complex code sections
- Validate your implementation approach

### During Code Review
- Check for style consistency
- Identify potential improvements
- Verify error handling is complete

### During Debugging
- Understand error stack traces
- Identify potential causes of bugs
- Get suggestions for fixes

### During Refactoring
- Plan architectural changes
- Ensure consistency across changes
- Generate updated documentation

---

## Feedback and Improvements

The Dev RAG Agent configuration can be customized in `.github/agents/dev-rag-agent.yml`:

- Adjust `temperature` for more/less creative responses (0.0-1.0)
- Modify `max_context_files` to use more/less context (1-20)
- Update `relevance_threshold` to be more/less selective (0.0-1.0)
- Add custom instructions for project-specific patterns
- Configure additional tools and capabilities

For questions or issues, refer to the main [README.md](README.md) in this directory.
