# Kimai MCP Usage Examples

This document provides practical examples of using the Kimai MCP server with AI assistants.

## Basic Time Tracking Workflow

### 1. Start Your Day - Check Active Timers

```
Assistant: Let me check if you have any active timers running.

Tool: timer
Parameters: {
  "action": "active"
}
```

### 2. View Recent Activities

```
Assistant: I'll show you your recent activities to help you quickly start a timer.

Tool: timer
Parameters: {
  "action": "recent",
  "size": 5
}
```

### 3. Start a Timer

```
Assistant: I'll start a timer for the API development task.

Tool: timer
Parameters: {
  "action": "start",
  "data": {
    "project": 15,
    "activity": 42,
    "description": "Implementing REST API endpoints",
    "tags": "development,backend"
  }
}
```

### 4. Stop the Timer

```
Assistant: I'll stop your current timer.

Tool: timer
Parameters: {
  "action": "stop",
  "id": 1234
}
```

## Project Management Examples

### List All Visible Projects

```
Tool: entity
Parameters: {
  "type": "project",
  "action": "list",
  "filters": {
    "visible": 1,
    "orderBy": "name",
    "order": "ASC"
  }
}
```

### Find Projects for a Specific Customer

```
Tool: entity
Parameters: {
  "type": "project",
  "action": "list",
  "filters": {
    "customer": 5,
    "visible": 1
  }
}
```

### Search Projects by Name

```
Tool: entity
Parameters: {
  "type": "project",
  "action": "list",
  "filters": {
    "term": "website",
    "visible": 3
  }
}
```

### Get Project Details

```
Tool: entity
Parameters: {
  "type": "project",
  "action": "get",
  "id": 15
}
```

## Timesheet Reporting Examples

### Today's Timesheet Entries

```
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "begin": "2024-01-15T00:00:00",
    "end": "2024-01-15T23:59:59",
    "user_scope": "self"
  }
}
```

### This Week's Entries

```
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "begin": "2024-01-15T00:00:00",
    "end": "2024-01-21T23:59:59",
    "orderBy": "begin",
    "order": "ASC",
    "user_scope": "self"
  }
}
```

### Billable vs Non-Billable Time

```
# Get billable entries
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "begin": "2024-01-01T00:00:00",
    "end": "2024-01-31T23:59:59",
    "billable": 1,
    "user_scope": "self"
  }
}

# Get non-billable entries
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "begin": "2024-01-01T00:00:00",
    "end": "2024-01-31T23:59:59",
    "billable": 0,
    "user_scope": "self"
  }
}
```

### Entries Ready for Export

```
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "exported": 0,
    "billable": 1,
    "size": 100,
    "user_scope": "self"
  }
}
```

## Activity Management Examples

### List All Global Activities

```
Tool: entity
Parameters: {
  "type": "activity",
  "action": "list",
  "filters": {
    "globals": "1",
    "visible": 1
  }
}
```

### List Activities for a Specific Project

```
Tool: entity
Parameters: {
  "type": "activity",
  "action": "list",
  "filters": {
    "project": 15,
    "visible": 1,
    "orderBy": "name"
  }
}
```

## Customer Examples

### List All Active Customers

```
Tool: entity
Parameters: {
  "type": "customer",
  "action": "list",
  "filters": {
    "visible": 1,
    "orderBy": "name",
    "order": "ASC"
  }
}
```

### Search for a Customer

```
Tool: entity
Parameters: {
  "type": "customer",
  "action": "list",
  "filters": {
    "term": "Acme",
    "visible": 3
  }
}
```

### Get Customer Details

```
Tool: entity
Parameters: {
  "type": "customer",
  "action": "get",
  "id": 5
}
```

## User and Team Management

### List All Users

```
Tool: entity
Parameters: {
  "type": "user",
  "action": "list",
  "filters": {
    "visible": 1,
    "orderBy": "username",
    "order": "ASC"
  }
}
```

### Get Current User Information

```
Tool: user_current
```

### Create a New Team

```
Tool: entity
Parameters: {
  "type": "team",
  "action": "create",
  "data": {
    "name": "Development Team",
    "color": "#3498db"
  }
}
```

### Add Member to Team

```
Tool: team_access
Parameters: {
  "action": "add_member",
  "team_id": 1,
  "user_id": 5
}
```

## Absence Management

### List Absences

```
Tool: absence
Parameters: {
  "action": "list",
  "filters": {
    "user": "5",
    "status": "all",
    "begin": "2024-01-01",
    "end": "2024-12-31"
  }
}
```

### Create an Absence

```
Tool: absence
Parameters: {
  "action": "create",
  "data": {
    "comment": "Annual vacation",
    "date": "2024-02-15",
    "end": "2024-02-20",
    "type": "holiday"
  }
}
```

### Get Absence Types

```
Tool: absence
Parameters: {
  "action": "types",
  "language": "en"
}
```

## Complex Workflows

### Daily Time Report

```
Assistant: I'll generate your daily time report.

1. First, let me check today's entries:
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "begin": "2024-01-15T00:00:00",
    "end": "2024-01-15T23:59:59",
    "user_scope": "self"
  }
}

2. Let me also check if you have any running timers:
Tool: timer
Parameters: {
  "action": "active"
}

3. Summary:
- Total entries today: 5
- Total time logged: 7h 30m
- Currently running: 1 timer (started 45m ago)
- Projects worked on: Website Redesign (3h), API Development (4h 30m)
```

### Quick Timer Switch

```
Assistant: I'll stop your current timer and start a new one for the meeting.

1. First, let me check your active timer:
Tool: timer
Parameters: {
  "action": "active"
}

2. Stopping the development timer:
Tool: timer
Parameters: {
  "action": "stop",
  "id": 1234
}

3. Starting the meeting timer:
Tool: timer
Parameters: {
  "action": "start",
  "data": {
    "project": 15,
    "activity": 99,
    "description": "Weekly team standup",
    "tags": "meeting,weekly"
  }
}
```

### Time Entry Correction

```
Assistant: I'll help you correct that time entry.

1. First, let me find the entry:
Tool: timesheet
Parameters: {
  "action": "list",
  "filters": {
    "begin": "2024-01-15T08:00:00",
    "end": "2024-01-15T12:00:00",
    "term": "API meeting",
    "user_scope": "self"
  }
}

2. Now I'll update it with the correct times:
Tool: timesheet
Parameters: {
  "action": "update",
  "id": 1235,
  "data": {
    "begin": "2024-01-15T09:00:00",
    "end": "2024-01-15T10:30:00",
    "description": "API planning meeting with team"
  }
}
```

### Create and Manage Timesheet Entry

```
Assistant: I'll create a new timesheet entry for your work yesterday.

Tool: timesheet
Parameters: {
  "action": "create",
  "data": {
    "project": 15,
    "activity": 42,
    "begin": "2024-01-14T09:00:00",
    "end": "2024-01-14T12:30:00",
    "description": "Database optimization and query improvements",
    "tags": "development,optimization,database"
  }
}
```

### Rate Management

```
# List customer rates
Tool: rate
Parameters: {
  "entity": "customer",
  "action": "list",
  "entity_id": 1
}

# Add a project rate
Tool: rate
Parameters: {
  "entity": "project",
  "action": "add",
  "entity_id": 15,
  "data": {
    "user": 5,
    "rate": 75.00,
    "internalRate": 65.00
  }
}
```

## Tips for AI Assistant Interactions

1. **Be specific with dates**: Always use ISO format (YYYY-MM-DDTHH:MM:SS) for date/time parameters

2. **Use search terms**: When looking for specific entries, use the `term` parameter in filters to search descriptions

3. **Leverage recent activities**: Use the timer tool with `action: "recent"` to quickly find frequently used project/activity combinations

4. **Check before starting**: Always check active timers with `timer` tool and `action: "active"` before starting a new timer to avoid multiple running timers

5. **Use user scope effectively**: For timesheet operations, use `user_scope: "self"` for your own entries, `"all"` for all users (requires permissions), or `"specific"` with a user ID

6. **Combine filters**: Use multiple filter parameters to narrow down results (e.g., specific project + date range + billable status)

7. **Entity tool versatility**: The `entity` tool handles all CRUD operations for projects, activities, customers, users, teams, tags, invoices, and holidays - just change the `type` parameter

8. **Action-based approach**: All consolidated tools use an `action` parameter to specify what operation to perform, making the interface consistent across all tools