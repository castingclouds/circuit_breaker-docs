---
weight: 3
bookFlatSection: true
---
# Circuit Breaker Executors

The Circuit Breaker executor system provides a flexible, DSL-driven approach to defining and running various types of execution environments. Each executor follows a consistent pattern while allowing for specialized configuration and behavior.

## Executor DSL

The executor DSL provides a declarative way to define:
- Required and optional parameters
- Type validations
- Custom validation rules
- Execution lifecycle hooks

### Basic Structure

```ruby
class MyExecutor < BaseExecutor
  executor_config do
    # Parameter definitions
    parameter :name,
      type: :string,
      required: true,
      description: 'Description of the parameter'

    # Validation rules
    validate do |context|
      # Custom validation logic
    end

    # Lifecycle hooks
    before_execute do |context|
      # Setup logic
    end

    after_execute do |result|
      # Cleanup or logging logic
    end
  end

  protected

  def execute_internal
    # Implementation
    @result = { status: 'completed' }
  end
end
```

### Parameter Types

The DSL supports the following parameter types:
- `:string`
- `:integer`
- `:array`
- `:hash`
- `:boolean`

Each parameter can be configured with:
- `required: true/false` - Whether the parameter must be provided
- `default: value` - Default value if not provided
- `description: 'text'` - Documentation for the parameter

## Available Executors

### Docker Executor

Runs containers with configurable images, commands, and environment.

```ruby
docker = DockerExecutor.new(
  image: 'nginx:latest',
  command: 'nginx -g "daemon off;"',
  environment: { 'PORT' => '8080' },
  volumes: ['/host/path:/container/path']
)
result = docker.execute
```

### NATS Executor

Manages workflow state and event distribution using NATS.

```ruby
nats = NatsExecutor.new(
  nats_url: 'nats://localhost:4222',
  petri_net: workflow_definition,
  workflow_id: 'custom-id'
)
result = nats.execute
```

### Assistant Executor

Interacts with AI assistants for natural language processing tasks.

```ruby
assistant = CircuitBreaker::Executors::AssistantExecutor.define do
  use_model 'qwen2.5-coder'
  with_system_prompt "You are a specialized assistant..."
  with_parameters temperature: 0.7, top_p: 0.9
  add_tools [AnalysisTool.new, SentimentTool.new]
end
