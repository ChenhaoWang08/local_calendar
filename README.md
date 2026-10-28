# PersonalCalendar

PersonalCalendar is a local-first personal calendar application for macOS. It brings schedules, tasks, email context, and a local large language model into one workspace, allowing users to search for information, plan their time, and create events through natural-language conversations.

The core goal is simple: let AI help manage time while keeping personal data on the user's own computer whenever possible.

## Key Features

### Local Calendar and Task Management

- View plans in daily, weekly, and timeline layouts.
- Create, edit, complete, and delete calendar events or tasks.
- Set start time, end time, all-day status, notes, tags, and completion status.
- Search schedules by date, time range, keyword, or tag.
- Find available time around existing classes, meetings, and personal plans.

### Local LLM Assistant

PersonalCalendar can connect to a language model running locally on the Mac. Users can interact with it conversationally, for example:

- “What important events do I have next week?”
- “Find my available time after 2:00 PM on Wednesday.”
- “Add a one-hour dentist appointment at 3:00 PM tomorrow.”
- “Did any recent course email mention the exam date?”
- “Use the time and location in this email to prepare an event draft.”

The model interprets intent and organizes the response. Calendar access, email search, and event creation are performed by constrained local tools. The model should never modify the database directly.

### Email Search and Calendar Integration

The application can use a restricted email connector to read information that is already authorized and visible to the user, then convert it into structured records that can be searched locally. Typical capabilities include:

- Search email by sender, subject, date, and keyword.
- Find meetings, assignments, deadlines, locations, and course arrangements mentioned in email.
- Reference relevant messages during a conversation without requiring manual copying.
- Generate a calendar-event draft from an email.
- Show the proposed time, title, location, and notes before anything is written to the calendar.

Email search and calendar operations are performed locally. The connector must not read saved browser passwords or bypass login and permission controls.

## Example Workflow

1. The user opens PersonalCalendar.
2. The application loads calendar events and tasks from the local SQLite database.
3. The email connector synchronizes message summaries within the authorized scope.
4. The user asks: “Find Professor Smith’s recent email about the final exam.”
5. The local LLM converts the request into an email-search tool call.
6. The application returns matching messages with their source and date.
7. The user continues: “Add the exam to my calendar and remind me to study two days in advance.”
8. The assistant prepares an event draft and a study-task draft.
9. The application writes the changes to the local calendar only after the user confirms them.

## Privacy Principles

- **Local-first:** Calendar data, tasks, conversation history, and the email index remain on the local machine by default.
- **Least privilege:** The email connector accesses only the information required for the current feature.
- **No password storage:** The application does not read or store browser passwords.
- **Explicit writes:** Before creating, editing, or deleting an event, the user should be shown exactly what will change.
- **Traceable sources:** When AI answers with email or calendar facts, it should identify where the information came from.
- **Optional integrations:** The basic calendar remains usable without the email connector or local LLM.

## Technical Summary

The known application implementation follows this technical direction:

- macOS 14 or later.
- Native Swift and SwiftUI interface.
- Local SQLite data storage.
- A local model runtime for Apple Silicon.
- MLX or another local model service as the LLM provider.
- An Outlook connector using Chrome Native Messaging.
- Layered modules for Domain, Application, Infrastructure, and UI concerns.

For product boundaries, architecture, and data flows, see [Overview](Overview.md).

## Existing Assets on This Mac

The following assets are still available on the current computer:

- Installed application: `/Applications/PersonalCalendar.app`
- Application data: `~/Library/Application Support/PersonalCalendar/`
- Main calendar database: `PersonalCalendar.sqlite3`
- Local models, runtime, and conversation records: `LocalAI/`
- Email connector state: `BrowserConnector/`
- Database backups: `Backups/`

These runtime assets are not the same as complete Swift source code. Back up the database and essential local configuration before moving, reinstalling, or cleaning the application.

## Project Status

PersonalCalendar version `0.2.0` remains installed, and its application data is still present. The original source repository was previously located at:

```text
/Users/zhangyixin/Desktop/class_schedule
```

That source directory no longer exists. The repository had no Git commit history and no configured remote repository. This directory now stores the recovered project documentation and can serve as the workspace for future reconstruction.

## Recommended Next Steps

1. Back up the existing SQLite database, conversation records, and connector configuration.
2. Reconstruct the Swift package using module and interface information retained in the installed `.app`.
3. Restore basic calendar reading, creation, and editing first.
4. Restore the email connector and local LLM tool calls next.
5. Add previews, confirmations, and audit records for every write operation.
6. Create a Git repository, establish a baseline commit as soon as the project builds, and configure an independent remote backup.

## Documentation

- [Overview](Overview.md): Product positioning, user scenarios, architecture, data flows, and security boundaries.
- `README.md`: Project entry point, major capabilities, runtime assets, and recovery status.

## License

No license has been selected. Before public distribution, the project should define its software license, review third-party model licenses, and establish code-signing and macOS notarization procedures.
