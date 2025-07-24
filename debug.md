# Lambda Function Debugging Demo

## Mutable Default Argument Bug

This Lambda function demonstrates a classic **Mutable Default Argument Bug** - an excellent choice for debugging demonstrations.

### What the Bug Does:
- The `process_user_data()` function has `metadata=[]` as a default parameter
- In Python, default arguments are evaluated only once when the function is defined
- The same list object is reused across all function calls, causing data to accumulate unexpectedly

### Demo Behavior:
When you call your Lambda function URL:
1. **First call**: `result1` will have 1 item
2. **Second call**: `result2` will have 2 items (including the first call's data!)
3. **Each subsequent Lambda invocation**: The list keeps growing within that container instance

### Why It's Great for Debugging Demos:
- **Subtle**: The bug isn't immediately obvious from reading the code
- **Realistic**: This is a common Python gotcha that catches even experienced developers
- **Observable**: You can see the bug in action by calling the function URL multiple times
- **Educational**: Shows how Lambda container reuse affects stateful bugs

### Testing the Bug:
- Call your function URL: `https://your-function-url/?user_id=john`
- Refresh the page multiple times - watch the list grow!
- The bug persists until the Lambda container is recycled

### The Fix (for your demo):
```python
def process_user_data(user_id, metadata=None):
    if metadata is None:
        metadata = []
    # ... rest of function
```

This creates a perfect debugging scenario where the issue becomes apparent through repeated function calls, making it ideal for demonstrating debugging techniques and Lambda behavior!

## Deployment Instructions

1. Deploy the SAM template:
   ```bash
   sam build && sam deploy --guided
   ```

2. Get the function URL from the outputs and test the bug by calling it multiple times

3. Observe how the metadata list grows with each invocation within the same container lifecycle