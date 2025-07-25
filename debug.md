# Lambda Function Debugging Demo

## Mutable Default Argument Bug

This Lambda function demonstrates a classic [Mutable Default Argument Bug](https://docs.python-guide.org/writing/gotchas/)

### What the bug does
- The `process_user_data()` function has `metadata=[]` as a default parameter.
- In Python, default arguments are evaluated only once when the function is defined.
- The same list object is reused across all function calls, causing data to accumulate unexpectedly.
- In case of Lambda this continues as long as the same execution environment is serving the requests.

### Demo behavior
When you call your Lambda function URL:
1. **First call**: `result` will have 1 item
2. **Second call**: `result` will have 2 items (including the first call's data!)
3. **Each subsequent Lambda invocation**: The list keeps growing within that container instance

### Testing
- Call your function URL: `https://your-function-url/?user_id=john`
- Refresh the page multiple times - watch the list grow!
- The bug persists until the Lambda container is recycled

### The Fix (for the debugging demo)
```python
def process_user_data(user_id, metadata=None):
    if metadata is None:
        metadata = []
    # ... rest of function
```

## Deployment instructions

1. Deploy the SAM template:
   ```bash
   aws cloudformation deploy --template-file template-bug.yaml --stack-name lambda-debug-demo --capabilities CAPABILITY_AUTO_EXPAND CAPABILITY_IAM CAPABILITY_NAMED_IAM
   ```

2. Get the function URL from the outputs and test the bug by calling it multiple times

3. Observe how the metadata list grows with each invocation within the same container lifecycle