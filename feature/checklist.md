# CSCSB - Project Workflow Checklist

## 1. Feature Assignment
- [ ] Feature assigned to developer

## 2. Issue Creation on GitHub
- [ ] Create a new issue on GitHub using the following format:
  ```
  Title: [Feature/Bug/Enhancement] Brief description of the issue

  Description:
  Detailed explanation of the feature, bug, or enhancement.

  Acceptance Criteria:
  - [ ] Criterion 1
  - [ ] Criterion 2
  - [ ] Criterion 3

  Unit Test Criteria:
  - [ ] Test case 1
  - [ ] Test case 2
  - [ ] Ensure >80% test coverage

  OpenAPI Specification:
  - [ ] Update OpenAPI spec with new/modified endpoints
  - [ ] Include request/response schemas
  - [ ] Document any new parameters or headers

  Additional Information:
  Any other relevant details, context, or resources.
  ```
- [ ] Assign the issue to the developer
- [ ] Add appropriate labels (e.g., 'feature', 'bug', 'enhancement')
- [ ] Link the issue to the relevant milestone
- [ ] Add the issue to the project board (if applicable)

## 3. Development Preparation
- [ ] Pull latest changes from the `rolling.dev` branch
  ```
  git checkout rolling.dev
  git pull origin rolling.dev
  ```
- [ ] Create a new feature branch using the following naming convention:
  ```
  git checkout -b dev.<issue number>/feature/feature-name
  ```
  Example: `git checkout -b dev.123/feature/user-authentication`
- [ ] Load and review all relevant project documents and guidelines

## 4. Development Process
- [ ] Implement the feature
- [ ] Write unit tests (aim for >80% coverage)
- [ ] Ensure code follows Go's official style guide and best practices
- [ ] Use descriptive, multi-word names for functions and variables
- [ ] Implement proper error handling
- [ ] Use linters (e.g., golangci-lint) to enforce code quality
- [ ] Document code using godoc standards:
  - Write a package comment for each package, describing its purpose and any important concepts
  - Add comments for all exported (capitalized) functions, types, and variables
  - Use complete sentences, starting with the name of the thing being described
  - Format examples as testable Go code
  - Include links to other relevant parts of the documentation where appropriate
  - Example:
    ```go
    // Package user provides functionality for managing user accounts.
    package user

    // User represents an individual user in the system.
    type User struct {
        ID       string
        Username string
        Email    string
    }

    // NewUser creates a new User with the given username and email.
    // It returns an error if the username or email is invalid.
    func NewUser(username, email string) (*User, error) {
        // Implementation details...
    }

    // Validate checks if the User's fields are valid.
    // It returns an error describing any validation failures.
    func (u *User) Validate() error {
        // Implementation details...
    }
    ```
- [ ] Update OpenAPI specification with new or modified endpoints
- [ ] Regularly commit changes with meaningful commit messages
  ```
  git commit -m "issue #<issue-number>: <brief description of changes>"
  ```
- [ ] Periodically load and review all project documents to ensure compliance with latest standards and guidelines

## 5. Branching Strategy
- [ ] Follow the branching strategy outlined in the document:
  - Use `dev.<issue number>/feature/` prefix for new features
  - Use `dev.<issue number>/bugfix/` prefix for bug fixes
  - Base feature branches on `rolling.dev`
  - Keep feature branches short-lived
  - Regularly pull changes from the base branch

## 6. Pull Request Creation
- [ ] Push the feature branch to GitHub
  ```
  git push origin dev.<issue number>/feature/feature-name
  ```
- [ ] Create a new Pull Request (PR) on GitHub using the following template:
  ```
  Title: [Feature/Bugfix/Enhancement] Brief description of the changes

  Closes #<issue-number>

  Description:
  Detailed explanation of the changes made and why.

  Acceptance Criteria:
  - [ ] Criterion 1 from the original issue
  - [ ] Criterion 2 from the original issue
  - [ ] Criterion 3 from the original issue

  Testing Instructions:
  1. Step-by-step instructions on how to test the changes
  2. Include any necessary setup or data requirements

  OpenAPI Specification Updates:
  - [ ] New endpoints added to OpenAPI spec
  - [ ] Existing endpoints updated in OpenAPI spec
  - [ ] Request/response schemas documented
  - [ ] New parameters or headers documented

  Additional Information:
  - List any dependencies or related PRs
  - Mention any potential impacts on other parts of the system

  Checklist:
  - [ ] Code follows project style guidelines
  - [ ] Unit tests added/updated (>80% coverage)
  - [ ] Documentation updated (if applicable)
  - [ ] OpenAPI specification updated
  - [ ] CI checks pass
  - [ ] Test case code coverage screenshot attached
  ```
- [ ] Add labels to the PR (e.g., 'feature', 'security')
- [ ] Assign reviewers (e.g., @teamlead, @securityexpert)
- [ ] Assign the PR to the developer (e.g., @developer1)
- [ ] Link the PR to the relevant milestone (e.g., v1.2)
- [ ] Link the PR to the project board (if applicable)
- [ ] Attach a screenshot of the test case code coverage to the PR

## 7. Code Review Process
- [ ] Wait for the project lead to review the PR
- [ ] Reviewers must:
  - [ ] Reject PRs with non-compiling or non-running code without further consideration
  - [ ] Ensure PRs have adequate comments and documentation (can approve but not merge if lacking)
  - [ ] Check for issue linking in commit messages
  - [ ] Verify that the code follows project style guidelines and best practices
  - [ ] Ensure that unit tests are present and cover the new/modified functionality
  - [ ] Validate that the OpenAPI specification has been updated (if applicable)
- [ ] Address any comments or requested changes
- [ ] Review the attached test case code coverage screenshot and ensure it meets the required threshold (>80%)
- [ ] Push new commits to the same branch if changes are required
  ```
  git commit -m "Address PR feedback: <brief description>"
  git push origin dev.<issue number>/feature/feature-name
  ```
- [ ] Re-request review after addressing all comments
- [ ] Ensure all project documents have been reviewed and followed

## 8. Continuous Integration
- [ ] Ensure all CI checks pass (e.g., tests, linting, build)
- [ ] Address any CI failures before merging

## 9. Merging
- [ ] Once approved and all checks pass, merge the PR into the target branch
- [ ] Verify that all required documentation and comments are present before merging
- [ ] Delete the feature branch after successful merge

## 10. Post-Merge Tasks
- [ ] Verify the changes are reflected in the target branch
- [ ] Update the issue status (e.g., close the issue or move to 'Done' on the project board)
- [ ] Update documentation if necessary
- [ ] Communicate the feature completion to the team
- [ ] Review and update any relevant project documents based on lessons learned or new insights gained during the development process

## 11. Release Preparation (when applicable)
- [ ] Create a release branch when preparing for a new release
- [ ] Update version numbers and changelog
- [ ] Perform final testing and bug fixes
- [ ] Merge the release branch into both `main` and `develop`
- [ ] Tag the release in `main`
  ```
  git tag -a v1.0.0 -m "Release version 1.0.0"
  git push origin v1.0.0
  ```

Remember to adapt this checklist as needed based on your specific project requirements and team workflows. Always refer to the most up-to-date project documents and guidelines throughout the development process.