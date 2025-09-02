Comprehensive Rust code quality analysis and reporting

Analyze a specific Rust project for compliance with best practices and provide detailed remediation guidance: #$ARGUMENTS

**Overview:**
Generate a comprehensive code quality report using the rust-codebase-analyzer agent to identify violations of Rust best practices in the specified project directory. Present findings organized by severity and provide actionable remediation steps.

**Analysis Process:**

1. **Initial Codebase Analysis**
   Use the `rust-codebase-analyzer` agent to perform a comprehensive analysis of the specified Rust project directory, checking for:
   - Code style and formatting violations
   - Performance anti-patterns  
   - Memory safety issues
   - Error handling problems
   - Architecture and design pattern violations
   - Documentation gaps
   - Testing coverage issues
   - Dependency management concerns

2. **Severity Classification**
   Organize all identified issues into clear severity categories:
   - **Critical**: Issues that could cause crashes, security vulnerabilities, or major performance problems
   - **Warning**: Code that works but violates best practices or could lead to future issues  
   - **Info**: Minor style issues or suggestions for improvement
   - **Documentation**: Missing or inadequate documentation

3. **Remediation Planning**
   For any identified issues, use sequential thinking (`mcp__sequential-thinking__sequentialthinking`) to analyze and develop specific remediation strategies:
   - Root cause analysis of each issue type
   - Step-by-step remediation plans
   - Priority recommendations for fixing issues
   - Potential risks of proposed changes
   - Alternative approaches when applicable

**Report Format:**

If no issues are found:
```
🎉 Excellent! Your Rust codebase fully complies with best practices. 
All code follows proper Rust idioms, error handling patterns, and architectural guidelines.
```

If issues are found, present a professional report structured as:

```
# Rust Code Quality Analysis Report

## Executive Summary
- Total issues found: [X]
- Critical: [X] | Warning: [X] | Info: [X] | Documentation: [X]

## Critical Issues
[List critical issues with file locations and brief descriptions]

## Warnings  
[List warning-level issues with file locations and brief descriptions]

## Info/Style Issues
[List minor issues with file locations and brief descriptions]

## Documentation Issues
[List documentation gaps and requirements]

## Remediation Recommendations

### High Priority Actions
[Specific steps to address critical issues]

### Medium Priority Actions  
[Steps to address warnings and improve code quality]

### Low Priority Actions
[Style improvements and documentation updates]

## Implementation Strategy
[Suggested order of operations and approach for fixes - NEVER include time estimates or schedules, only logical priority and dependencies]

```

**Important Constraints:**
- NEVER make any code changes - this is strictly a reporting and analysis command
- Focus only on Rust best practices and code quality
- Provide specific file locations and line numbers when possible
- Ensure remediation suggestions are actionable and specific
- Use professional, constructive language in all reports
- ABSOLUTELY NO time estimates, schedules, or timeline suggestions in any part of the report