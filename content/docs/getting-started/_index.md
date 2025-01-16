---
weight: 2
bookFlatSection: true
---
## Installation

Add this line to your application's Gemfile:

```ruby
gem 'circuit_breaker-wf'
```

And then execute:
```bash
$ bundle install
```

Or install it yourself as:
```bash
$ gem install circuit_breaker-wf
```

## Basic Usage

### Action-Rule Data Flow

Circuit Breaker provides a powerful mechanism for passing data between actions and rules during workflow transitions. Here's how it works:

1. **Actions with Named Results**
```ruby
flow :draft >> :pending_review, :submit do
  actions do
    # Execute action and store result with key :clarity
    execute analyzer, :analyze_clarity, :clarity
  end
  policy all: [:valid_clarity]
end
```

2. **Accessing Results in Rules**
```ruby
CircuitBreaker::Rules::DSL.define do
  rule :valid_clarity do |token|
    # Retrieve stored result using the same key
    clarity = context.get_result(:clarity)
    clarity && clarity[:score] >= 70
  end
end
```

### Data Flow Process
- Actions are executed first during a transition
- Results are stored in an action context using the specified key
- Rules can access these results through the same key
- This enables rules to validate based on action outputs

This pattern allows for:
- Clean separation between action execution and rule validation
- Reusable actions with different validation rules
- Complex rule chains based on multiple action results
- Clear data flow tracking during transitions
