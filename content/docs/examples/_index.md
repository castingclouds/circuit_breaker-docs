---
weight: 4
bookFlatSection: true
title: "Examples"
---

# Document Workflow Example

This example demonstrates how to use Circuit Breaker to implement a document workflow system with AI-powered analysis. It showcases a declarative DSL for defining workflows, rules, validations, and AI-powered document analysis.

## Workflow Overview

The document workflow system implements a document review and approval process with the following states:
- `draft`: Initial state for new documents
- `pending_review`: Document submitted for review
- `reviewed`: Review completed with comments
- `approved`: Document approved by manager
- `rejected`: Document rejected with reasons

## Key Features

### 1. Unified Rules System
- Declarative rule definitions using DSL
- Complex rule chains with AND/OR logic
- Built-in validation helpers
- Clear error reporting
- Rule reusability across transitions

### 2. Workflow Management
- Intuitive state transition syntax (`from >> to`)
- Policy-based transitions with rule chains
- Comprehensive history tracking
- Event handling and state management
- Automatic validation during transitions

### 3. Document Rules
- Document validation rules:
  - `valid_reviewer`: Ensures reviewer is assigned
  - `valid_review`: Validates review comments
  - `valid_approver`: Checks approver assignment
  - `is_admin_approver`: Verifies approver permissions
  - `valid_word_count`: Checks document length
  - `valid_external_url`: Validates external references

### 4. AI-Powered Document Analysis
- Content quality and structure assessment
- Sentiment and tone analysis
- Automatic context detection
- Improvement suggestions
- Word count validation

## Implementation

### 1. Define Document Rules

```ruby
rules = DocumentRules.define do
  rule :valid_reviewer do |token|
    token.reviewer_id.present?
  end

  rule :valid_review do |token|
    token.reviewer_comments.present?
  end

  rule :is_admin_approver do |token|
    token.approver_id&.start_with?('admin_')
  end
end
```

### 2. Create Workflow

```ruby
workflow = CircuitBreaker::WorkflowDSL.define(rules: rules) do
  states :draft, :pending_review, :reviewed, :approved, :rejected

  flow(:draft >> :pending_review)
    .transition(:submit)
    .policy(rules: { all: [:valid_reviewer] })

  flow(:pending_review >> :reviewed)
    .transition(:review)
    .policy(
      rules: {
        all: [:valid_review],
        any: [:is_high_priority, :is_urgent]
      }
    )

  flow(:reviewed >> :approved)
    .transition(:approve)
    .policy(
      rules: {
        all: [:valid_approver, :valid_review, :is_admin_approver],
        any: [:valid_external_url, :valid_word_count]
      }
    )
end
```

### 3. Process Document

```ruby
# Create document
token = Examples::DocumentToken.new(
  title: "Project Proposal",
  reviewer_id: "user_123",
  reviewer_comments: "Looks good, minor revisions needed",
  approver_id: "admin_456"
)

# Initialize workflow
workflow = DocumentWorkflow.new(token)

# Submit for review
workflow.submit

# Review document
workflow.review

# Approve document
workflow.approve
```
