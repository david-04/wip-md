---
name: wip-specify
description: Never use this skill unless explicitly requested by the user
license: MIT
user-invocable: true
---

# General instructions

Your task is to create or update a specification document called `WIP.md`. You will be provided with some background information followed by steps on how to create or update the file. Do not create or update `WIP.md` in a way that you _think_ is right. Always follow the instructions very closely. Do not make any code changes either. Your only task is to create or update `WIP.md`.

Whenever you receive user input, always reload file `WIP.md`. It might have been modified in the background.

Important: Whenever the user provides actionable input, you MUST immediately incorporate this input into the `Requirements` and `Tasks` sections of `WIP.md` by following the steps below. You must incorporate input even if the `Tasks` and `Requirements` sections are already populated. Never defer or ignore actionable input. Do not wait for further clarification, confirmation, or additional input from the user.

# Structure of the `WIP.md`

The document `WIP.md` contains 3 sections:

1. `Overview`
    - The `Overview` section contains a brief summary of the capabilities added by the feature.
    - It's formatted as a single paragraph with only a few short sentences.

2. `Requirements`
    - The `Requirements` section contains the requirements. It describes the expected behavior in different scenarios. A requirement can be:
        - A capability or process step (e.g.: "Load and parse the CSV file")
        - An expected behavior (e.g.: "Raise an error when the file does not exist")
        - A technical hint (e.g.: "Store user data in the component's state")
    - Requirements are grouped into a handful of categories, which are formatted as a bullet point list.
    - Under each category list item, there is a nested bullet point list with the specific requirements that belong into the category.

3. `Tasks`
    - The `Tasks` section lists all the steps that are needed to implement the feature. Each step describes one or more code changes.
    - The `Tasks` section is formatted as a bullet point list. Each list item has the format (`- <EMOJI> Task <NUMBER>: Task description`). The `<EMOJI>` can be one of the following:
        - ⏸️ means that the implementation has not started yet (`- ⏸️ Task <NUMBER>: Task description`)
        - ⌛ means that the task is currently being implemented (`- ⌛ Task <NUMBER>: Task description`)
        - ✅ means that the task has been implemented (`- ✅ Task <NUMBER>: Task description`)
    - Tasks are numbered sequentially (`Task 1`, `Task 2`, ...).
    - Under each top-level list item, there can be a nested bullet point list that describes the task in more detail. Nested bullet points are not numbered.

This is an example of a `WIP.md` file:

```
# WIP

## Overview

Read a CSV file, split rows and columns, validate all data, and return the parsed data to the main program.

## Requirements

- The application can read a CSV file
  - The comma (`,`) is used as the field separator (not configurable)
  - Throws an error if the file does not exist
- The application can parse the CVS data
  - Columns are read in this order: name, date of birth, address
  - Throws an error when a row has too many or too few columns

## Tasks

- ⏸️ Task 1: Create `src/import/readCsv.ts` and add interface `CsvData` to represent the read data
  - `CsvData` contains the fields name, dateOfBirth and address
  - All fields are read-only
- ⏸️ Task 2: In `src/import/readCsv.ts`, add a new function `readCsv` that reads the CSV file into memory
  - Read the file via Node's readFileSync function
- ⏸️ Task 3: Handle "file not found" error in `readCsv`
```

# Instructions for creating or updating `WIP.md`

Whenever you receive user input, follow these steps to create or update the specification document `WIP.md`:

1. Create or read `WIP.md`
    - If the project's root directory contains a file called `WIP.md`, read its content.
    - If the project's root directory does not contain a file called `WIP.md`, create it with the following content (don't add anything else to it):

        ```
        # WIP

        ## Overview

        ## Requirements

        ## Tasks
        ```

    - The absence of `WIP.md` is a clear signal that you are starting a new specification. In this case you MUST ask clarifying questions before populating the file.

2. Ask clarifying questions
    - You must only ask clarifying questions if any of the following is true:
      a. `WIP.md` does not exist (or you have just created it in step 1)
      b. the `Requirements` section in `WIP.md` is empty (contains no bullet points)
      c. the user explicitly asked you to ask them questions
    - If any of the above is true, you MUST ask questions before incorporating any user input in the `Requirements` and `Tasks` sections of `WIP.md`.
    - If you have established that you do not need to ask clarifying questions (because `WIP.md` already exists and the `Requirements` section is not empty, and the user did not explicitly request questions), do NOT ask clarifying questions. Instead, immediately incorporate all actionable user input into the `Requirements` and `Tasks` sections as described below.
    - Review the user's input and everything that's currently documented in `WIP.md`. Identify aspects that are missing, incomplete, ambiguous, or unclear. Think about possible edge cases and error scenarios. Pick the three most significant/important questions that should be clarified. Do not pick questions that are already answered in `WIP.md`.
    - Obtain user input:
        - Ask the user 4 questions. Include the 3 most significant/important questions identified above, as well as a 4th question: "Ask more questions?" (use this exact phrase as the question and provide the answer options "Yes" and "No").
        - Always use the tool for asking the user questions (it might be called something like "ask questions" or "ask user questions").
        - Always provide multiple predefined answers (single or multiple choice) for the user to pick from.
        - Always include a free-text field as an extra answer option, so that the user can provide a different answer altogether.
    - If the user answers the last question ("Ask more questions") with "Yes", repeat the steps above. That is, identify the next 3 most significant/important questions and ask the user again. Repeat this cycle until the user indicates that no more questions should be asked. Do not ask the user for confirmation before repeating. Immediately ask the next questions.
    - Important: If the user's answer to the "Ask more questions?" prompt is "Yes", do NOT proceed to incorporating answers. Instead, continue the Q&A loop immediately (without awaiting further confirmation from the user). Only stop asking questions when the user answers "No" to the "Ask more questions?" prompt.
    - If the user answers the last question ("Ask more questions") with "No", stop asking questions. Immediately proceed to the next step and update the `Requirements` and `Tasks` sections of `WIP.md` with the user's answers. Do not display any message about incorporating answers or completing the update until the answers have actually been written to `WIP.md` and the file has been saved. Never stop or yield until the answers are fully incorporated and the file is updated.
    - Treat answers provided via the question/answer tool as regular user input that must be incorporated automatically. In particular, whenever the user replies to clarifying questions (either immediately after they were asked or later in the chat), you MUST:
        - Immediately read the current `WIP.md`, incorporate the answers into the `Requirements` section (see step 4) and then invoke the subagent to update the `Tasks` section (see step 5).
        - Save `WIP.md` after each incorporation cycle before returning a message to the user claiming the document has been updated.
        - Do not wait for an additional explicit "save" or confirmation from the user - answers to your questions are actionable input and must be written into `WIP.md` automatically.
    - When incorporating answers, always try to treat them as regular requirements and put them into the appropriate group within the `Requirements` section. In rare cases, where an answer does not constitute a requirement, add it to a special top-level requirement group called `Clarifications` (if not already present). Summarize the answer as a statement (not in Q&A format).

3. Populate or update the `Overview` section in `WIP.md`
    - If the `Overview` section already contains a summary of the feature, compare the summary with the user input and the current requirements in the `Requirements` section. If `Overview` section is not empty, and if it is still a good description of the feature, skip this step.
    - Populate or update the `Overview` section with a high-level summary of the capabilities added by the feature.
    - The summary must be a single paragraph with only a few sentences. Use short sentences that are easy to read. Omit low-level details like field or identifier names.
    - Ensure that the header (`## Overview`) is preserved. Never remove it and never add a duplicate header.
    - Save file `WIP.md`.

4. Populate or update the `Requirements` section in `WIP.md`
    - Identify all requirements
        - Consider all input that the user has provided, including the very first message when the skill is invoked, and any subsequent messages or answers to clarifying questions. Also consider all requirements that are currently described in the `Requirements` section of `WIP.md`.
        - Requirements can be functional requirements (features and expected behaviors) or technical instructions (e.g. how to implement something, or how to name files and functions). Treat all of them as requirements that are relevant for the `Requirements` section.
        - User input might include sample data (like a JSON snippet) or instructions on how to structure/implement the code. Treat all of this input as requirements and make sure to preserve it in the `Requirements` section.
        - Only consider requirements that are directly derived from user input (including answers to questions) or already documented in the `Requirements` section. Do not treat general instructions from files like `AGENTS.md` as requirements.
    - Populate the `Requirements` section
        - If the `Requirements` section is empty, create it from scratch. If it already contains requirements, update them as required.
        - Make sure to preserve all pre-existing requirements. Only remove requirements that have been superseded by newer requirements (or that the user asked to remove).
        - When new requirements extend or partially overlap with pre-existing requirements, try to update the pre-existing requirements to include the new requirements as well. For example, try to phrase old requirements wider/more general. Avoid having multiple bullet points that describe the same or very similar requirement or behavior.
        - When updating the `Requirements` section, always preserve the structure of top-level bullet points with nested bullet point lists. You can add, update, merge, or re-arrange/re-group the top-level bullet points as needed, but do not just remove them. The `Requirements` section must always start with a non-indented top-level bullet point, and there must be a top-level bullet point for each group.
        - Group the requirements into at least 3 groups/categories. A group/category could be a smaller piece of the feature (e.g. "Read file" or "Validate file contents") or a cross-functional concern (e.g. "Error handling").
        - When updating the `Requirements` section with new requirements, review the grouping and regroup the requirements into different/new categories if necessary. The groups should be somewhat balanced in size. Avoid groups that are much larger or much smaller than the other groups.
        - Ensure that all user input (including answers to questions and details like names and identifiers) is captured in the `Requirements` section. Preserve as much detail as possible.
        - If the user provides you with implementation instructions (e.g. in which files to implement certain functionality, or how to name files or functions), keep them in a group called `Implementation instructions`.
        - Format the groups and requirements as nested bullet point lists. The top-level bullet point list contains all the groups/categories. Under each group/category list item, create a nested bullet point list with the specific requirements that relate to this group/category.
        - Ensure that the header (`## Requirements`) is preserved. Never remove it and never add a duplicate header.
    - Save file `WIP.md`.

5. Populate or update the `Tasks` section in `WIP.md`
    - Unless instructed otherwise by the user, do not consider testing when creating or updating the `Tasks` section. If applicable, test will be implemented automatically, even when not mentioned.
    - If the user explicitly asked for tests to be added, include them in the same task that also covers the implementation. Never split implementation and test into two separate tasks.
    - Come up with a draft implementation plan
        - Run a subagent, to make a plan of which code changes are required.
        - Explicitly tell the subagent that its job is planning only. It should produce a list of implementation tasks and it must not write any code, modify files, or attempt to implement the feature itself.
        - Provide the subagent with the description of the feature (from the `Overview` section) and the current requirements (from the `Requirements` section).
        - Additionally, pass on all user input (including answers to questions).
        - If the `Tasks` section is already populated, pass it on to the subagent and ask it to have a look around the code base and advise how the current plan needs to be updated.
        - If the `Tasks` section is currently empty, ask the subagent to look at the code base and identify all code changes that are needed to implement the requirements.
        - Instruct the subagent to only consider the input you've provided it with. The subagent should not treat general guidelines from instructions files like `AGENTS.md` as requirements.
        - Wait until the subagent returns its findings. Do not proceed while the subagent is still running.
    - Incorporate the implementation plan into the `Tasks` section
        - Always update the `Tasks` section in-place. Never prepend, append, or create a duplicate `Tasks` section. The file must contain exactly one `## Tasks` header and one list of tasks. Before saving, check for and remove any duplicate or extra `## Tasks` headers and their content, ensuring only one `Tasks` section exists in the file. Never put the `Tasks` section into the `Requirements` section. The `Tasks` section must always be the last section in `WIP.md`.
        - Ensure to incorporate all details that the subagent has provided.
        - Make sure that each implementation task has a good size, so that it can later be implemented by a subagent in one go. As a rule of thumb, there should usually be between 3 and 6 tasks. But you can create fewer or more tasks if the feature is very small or very large.
        - Format each task as a bullet point list item (`- <EMOJI> Task <NUMBER>: Task description`). You can add a nested bullet point list (`- Detail description`) under each task, to describe more code changes more granularly and with more details.
        - For pre-existing tasks, preserve the `<EMOJI>` that's already there. When adding new tasks, use ⏸️ as the emoji (`- ⏸️ Task <NUMBER>: Task description`).
        - Do not repeat detailed expected behavior that's already described in the `Requirements` section. Only summarise which functionality is added. For example, simply state: "Implement validation", rather than repeating individual validation rules that are already described in the `Requirements` section.
        - Ensure that each task states the precise relative path of all files that it creates/updates. Surround the paths with backticks, so that Markdown renders them as code.
        - Ensure that each task states the precise name of every function that it creates/changes. Surround the names with backticks, so that Markdown renders them as code.
        - Ensure that each task states the precise name of every class, interface or type that it creates/changes. Surround each name with backticks, so that Markdown renders them as code. Only include the name and type. Do not include the names or data types of individual attributes.
        - Ensure that all tasks are ordered so that they can be implemented one after the other. Make sure that no task depends on functionality that is only added by a task further down the list. Re-order tasks if necessary.
    - Whenever you have updated the `Tasks` section, you MUST verify that the formatting is correct:
        - The `Tasks` section must be the last section in `WIP.md`. There must be exactly one `Tasks` section.
        - Renumber all top-level tasks so that the task numbers are unique, sequential, and consecutive, starting from 1 (e.g., `- ⏸️ Task 1: ...`, `- ⏸️ Task 2: ...`). Never leave behind duplicate or out-of-order task numbers.
        - Make sure that nested bullet point lists do not contain emojis or task numbers (`- Description`).
    - Save file `WIP.md`.
    
6. Advice the user on the next steps
    - Display the following text to the user:

        ```
        Next steps:

        - Let me know what you want changed
        - I can cask you more clarifying questions
        - To purge the context, start a new chat session with `/wip-specify`
        - To implement the feature, start a new chat session with `/wip-implement`
        ```

    - Display the text exactly as given. Do not modify the message. Don't add other options or suggestions. Don't summarize what you've done and don't display the contents of `WIP.md`.
    - Don't ask the user if you should start to implement the feature either. Your only task is to create or update file `WIP.md`.
    - Do not add the above message (or anything else) to `WIP.md`.

7. If you have receive any kind of user input (either directly or as answers to questions), and you have not incorporated it into `WIP.md`, explain why you did not add the new information to the document.

8. Whenever the user continues the chat
    - Always read file `WIP.md` again. Do not re-use the content that's already in the context. The user might have modified the file in the background.
    - Whenever you receive input from the user (directly or as answers to questions that you ask), make sure to always incorporate this input into the `Requirements` and `Tasks` sections of `WIP.md`, by following the steps described above.
    - If the input is an answer to clarifying questions you previously asked, treat it as high-priority actionable input: immediately update `WIP.md` (Requirements and Tasks), run the subagent to refresh the implementation plan, save the file, and only then respond to the user. Do not ask for an extra confirmation before saving these answers into `WIP.md`.

---
