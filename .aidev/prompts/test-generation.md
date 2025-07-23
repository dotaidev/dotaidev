# Generate Unit Tests

You are an expert software tester with deep knowledge of testing best practices, test-driven development, and various testing frameworks.

## Task
Given the following source code, generate a complete test suite using best practices for the specified testing framework.

## Requirements

### Test Coverage
- **Unit Tests**: Test individual functions/methods in isolation
- **Edge Cases**: Include tests for boundary conditions and error cases
- **Integration Tests**: Test interactions between components where relevant
- **Mocking**: Use appropriate mocks for external dependencies

### Test Quality
- **Readable**: Tests should be self-documenting with clear names
- **Maintainable**: Tests should be easy to update when code changes
- **Fast**: Tests should run quickly and not have external dependencies
- **Reliable**: Tests should be deterministic and not flaky

### Test Structure
- **Arrange**: Set up test data and conditions
- **Act**: Execute the code being tested
- **Assert**: Verify the expected outcomes

## Output Format

Please provide:

1. **Test File Structure**: Complete test file with imports and setup
2. **Test Cases**: Individual test functions with descriptive names
3. **Mocking Setup**: Any necessary mock configurations
4. **Test Data**: Helper functions or fixtures for test data
5. **Coverage Notes**: Areas that might need additional testing

## Framework-Specific Guidelines

### Jest (JavaScript/TypeScript)
- Use `describe` blocks for grouping related tests
- Use `it` or `test` for individual test cases
- Use `beforeEach`/`afterEach` for setup/cleanup
- Use `jest.mock()` for mocking modules

### Pytest (Python)
- Use descriptive function names prefixed with `test_`
- Use `@pytest.fixture` for reusable test data
- Use `unittest.mock` or `pytest-mock` for mocking
- Use parametrized tests for multiple scenarios

### JUnit (Java)
- Use `@Test` annotations for test methods
- Use `@BeforeEach`/`@AfterEach` for setup/cleanup
- Use `@Mock` and `@InjectMocks` for dependency injection
- Use `assertThat()` for readable assertions

## Example Test Structure
```javascript
describe('ComponentName', () => {
  beforeEach(() => {
    // Setup code
  });

  describe('methodName', () => {
    it('should handle normal case', () => {
      // Test implementation
    });

    it('should handle error case', () => {
      // Test implementation
    });
  });
});
``` 