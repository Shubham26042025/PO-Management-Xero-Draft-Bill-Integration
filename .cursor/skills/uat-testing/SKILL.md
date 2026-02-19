---
name: uat-testing
description: Automatically execute User Acceptance Testing based on FRD and sprint plan. Use when development is complete, when running UAT, or when validating features against acceptance criteria.
---

# UAT Testing

Automatically execute User Acceptance Testing (UAT) based on Functional Requirements Document (FRD), sprint plan, and User Acceptance Criteria (UAC).

## Prerequisites

Before running UAT, ensure:

1. **FRD is available** in `BA Docs/Functional Requirement Document/`
2. **Sprint plan** identifies deliverables for the sprint
3. **UAC CSV** is available in `BA Docs/User Stories and Acceptance Criteria/`
4. **Application is deployed** and accessible (staging/UAT environment)
5. **Credentials are configured** (stored securely, not hardcoded)
6. **Test data** is prepared or available

## Phase 1: Test Planning

### Read Required Documents

1. **Read FRD** for the sprint:
   - Extract functional requirements
   - Identify modules/features in scope
   - Note user roles, workflows, and validations
   - Identify integrations and dependencies

2. **Read Sprint Plan**:
   - Confirm sprint number
   - List deliverables/modules included
   - Note out-of-scope items (exclude from testing)

3. **Read UAC CSV**:
   - Parse acceptance criteria for sprint modules
   - Extract test scenarios (positive, negative, edge cases)
   - Map to user roles and workflows

### Create Test Matrix

Map each UAC row to test scenarios:

- **Module**: Which feature/module
- **User Role**: AS A/AN from UAC
- **Test Scenario**: I want to + So That
- **Acceptance Criteria**: Expected outcome
- **Test Type**: Positive, Negative, Edge Case, Role-based, Integration

## Phase 2: Test Execution Strategy

### Determine Application Type

- **Web Application**: Use browser automation (Playwright, Selenium, or browser MCP)
- **API Application**: Use API testing tools (Postman, REST clients, or HTTP requests)
- **Hybrid**: Combine browser and API testing

### Test Execution Approach

1. **Sequential Testing**: Execute tests in logical workflow order
2. **Dependency Mapping**: Test prerequisites first (e.g., login, data setup)
3. **Isolation**: Each test should be independent where possible
4. **Cleanup**: Reset state between test runs if needed

## Phase 3: Test Execution

### For Web Applications

Use browser automation to:

1. **Navigate** to application URL
2. **Authenticate** using configured credentials
3. **Execute workflows** as per FRD:
   - Navigate through UI flows
   - Fill forms with test data
   - Submit actions
   - Verify outcomes
4. **Validate** against acceptance criteria:
   - Check UI elements visibility/state
   - Verify data displayed correctly
   - Confirm success/error messages
   - Validate business rules

### For API Applications

Use API testing to:

1. **Authenticate** and obtain tokens
2. **Execute API calls** per FRD workflows:
   - POST/PUT for create/update operations
   - GET for read operations
   - DELETE for delete operations
3. **Validate responses**:
   - Status codes match expected
   - Response body contains expected data
   - Error handling works correctly
   - Business validations enforced

### Test Coverage Per Module

For each module in sprint scope:

- ✅ **Positive Tests**: Happy path scenarios
- ✅ **Negative Tests**: Invalid inputs, error handling
- ✅ **Edge Cases**: Boundary conditions, empty/null values
- ✅ **Role-based**: Permissions and access control
- ✅ **Validations**: Business rules and constraints
- ✅ **Integrations**: Third-party syncs, data flows

## Phase 4: Validation Against UAC

For each acceptance criteria:

1. **Execute** the test scenario
2. **Capture** actual result
3. **Compare** with expected outcome from UAC
4. **Record** pass/fail status
5. **Document** evidence (screenshots, API responses, logs)

### Binary Outcomes

If UAC specifies Yes/No outcome:
- **Yes**: Feature/behavior must be present/working
- **No**: Feature/behavior must be absent/blocked

## Phase 5: Test Reporting

Generate comprehensive test report:

### Report Structure

```
# UAT Test Report - Sprint [X]

## Test Summary
- Total Test Cases: [X]
- Passed: [X]
- Failed: [X]
- Blocked: [X]
- Pass Rate: [X]%

## Test Execution Details

### Module: [Module Name]
- Test Case: [UAC Reference]
- User Role: [Role]
- Scenario: [Description]
- Status: ✅ Pass / ❌ Fail / ⚠️ Blocked
- Evidence: [Screenshot/Response]
- Notes: [Any observations]

## Failed Tests
[List failed tests with details]

## Blocked Tests
[List blocked tests with reasons]

## Recommendations
- [Issues found]
- [Next steps]
```

### Evidence Collection

- **Screenshots**: For UI validations
- **API Responses**: For API validations
- **Logs**: Error logs, console output
- **Test Data**: Inputs used, outputs received

## Phase 6: Post-Testing Actions

1. **Document Results**: Save test report to `BA Docs/Test Reports/`
2. **Flag Issues**: Create issue tickets for failed tests
3. **Communicate**: Share results with development team
4. **Retest**: After fixes, rerun failed test cases

## Automation Scripts

### Example: Web Application Test Flow

```python
# Pseudo-code structure
def execute_uat():
    # 1. Read FRD and UAC
    frd = read_frd(sprint_number)
    uac = read_uac_csv(sprint_number)
    
    # 2. Create test scenarios
    test_scenarios = map_uac_to_tests(uac, frd)
    
    # 3. Execute tests
    results = []
    for scenario in test_scenarios:
        result = execute_test(scenario)
        results.append(result)
    
    # 4. Generate report
    generate_report(results)
```

### Example: API Test Flow

```python
# Pseudo-code structure
def execute_api_uat():
    # 1. Authenticate
    token = authenticate(credentials)
    
    # 2. Execute API tests per UAC
    for uac_row in uac_data:
        response = call_api(uac_row.endpoint, uac_row.method, uac_row.data)
        validate_response(response, uac_row.expected_outcome)
    
    # 3. Generate report
    generate_api_report(results)
```

## Best Practices

1. **Test Data Management**: Use realistic test data, avoid hardcoding
2. **Error Handling**: Capture and report errors clearly
3. **Retry Logic**: For flaky tests, implement retry mechanisms
4. **Parallel Execution**: Run independent tests in parallel when possible
5. **Environment Validation**: Verify test environment before starting
6. **Credential Security**: Never commit credentials, use environment variables or secure storage

## Troubleshooting

- **Application not accessible**: Verify URL, network, deployment status
- **Authentication fails**: Check credentials, token expiration
- **Tests timeout**: Increase timeout, check application performance
- **Flaky tests**: Add retry logic, investigate root cause
- **Missing test data**: Prepare test data setup scripts

## Integration with Development

- Run UAT after development completion
- Share results with development team
- Track fixes and retest
- Maintain test history per sprint
