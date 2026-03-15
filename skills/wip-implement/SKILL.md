---
name: wip-implement
description: Never use this skill unless explicitly requested by the user
license: MIT
user-invocable: true
---

Your task is to orchestrate and instruct subagents to implement a feature described in the specification document `WIP.md` in the project's root directory. You will be provided with information about the structure of document `WIP.md`, followed by steps that you have to take to complete your task.

- As you follow the steps below, you must only ever update file `WIP.md`. Do not update any other file. All other work needs to be delegated to subagents. Do not make code changes yourself.
- The task requires you to complete multiple steps. Whenever you have completed one step, immediately proceed to the next step automatically. Do not wait for user input or confirmation. Your goal is to run autonomously until all your tasks have been fully completed.

# Structure of the `WIP.md`

The document `WIP.md` describes a feature that the user wants to build. It contains 3 sections:

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

# Your instructions

Your task is to run subagents to implement individual tasks, or to complete final reviews and clean-ups. Follow these steps to complete your task:

1. Read file `WIP.md` in the project's root directory. If the file does not exist, abort the process and tell the user that they must create file `WIP.md` first, by using the skill `wip-specify`.

2. Determine if tests need to be implement. Pick the first matching option from this list (and do not consider subsequent options after finding the first match):
    - If the user explicitly asked you to not implement tests => do not implement tests
    - If the user explicitly asked you to implement tests => implement tests
    - If `WIP.md` contains explicit instructions or requirements that refer to tests => implement tests
    - If `WIP.md` contains explicit instructions to not add tests => do not implement tests
    - If general instructions (like `AGENTS.md`) state that this project does not uses tests => do not implement tests
    - If general instruction files (like `AGENTS.md`) state that this project uses tests => implement tests
    - If you have been provided with instructions on how to run tests => implement tests
    - If the project contains test files (e.g. in a `test` folder or with file names like `module.spec.rb` or `module.test.ts`) => implement tests
    - If the project has a test framework (like `jest` or `rspec`) installed => implement tests
    - If none of the above applies => do not implement tests

3. Determine which tasks to implement
    - Look at the `Requirements` section of `WIP.md` and identify all tasks that need to be implemented.
    - If the user has requested that only specific tasks are implemented (specified by the task numbers), then verify that tasks with these numbers exist in `WIP.md`.
    - If the user has not requested specific tasks, you need to implement all tasks with emoji ⏸️ (format: `- ⏸️ Task <NUMBER>: Task description`) or ⌛ (format: `- ⌛ Task <NUMBER>: Task description`). Skip tasks with emoji ✅ (format: `- ✅ Task <NUMBER>: Description`), as they have already been implemented.
    - The selected tasks must be implemented in the right order as they are numbered, and as they occur from top to bottom in file `WIP.md`.
    - Once you have determined the relevant tasks, display a message to the user. List all the tasks that you've found in `WIP.md` and, for each task, state if you need to implement it or not (and why).
    - After displaying the message, immediately continue to the next step to go through and implement all tasks. Do not wait for user input or confirmation.

4. Go through all tasks that you've determined in the previous step. For each task, follow these steps:
    - Update `WIP.md` and set the task's emoji in the `Tasks` section to ⌛ (format: `- ⌛ Task <NUMBER>: Description`). Only update this one line and only change the emoji. Do not make any other changes (neither to this line, nor to any other line in the file). Then save file `WIP.md` immediately.

    - Run a subagent to implement the current task. Do not specify an agent name. Do not use an agent that's specialised in planning or exploration. The agent must be capable of immediately implementing the required code changes. Provide the subagent with the following instructions (exactly as they are given here, do not modify them):

        ```
        You are a subagent tasked with implementing a part of a feature that's described in a specification document called `WIP.md`. The document contains 3 sections. The `Overview` section contains a brief summary of the feature. The `Requirements` section lists the required functionality and expected behaviours. The `Tasks` section contains coding tasks that, together, implement the whole feature. Your task is to implement exactly one task from the `Tasks` section. You will receive instructions of which task to implement. You will also be advised if you need to implement tests. To complete your mission, follow these steps:

        - Read file `WIP.md` from the project's root directory and review its content.
        - In the `Tasks` section, find and review the task that you've been assigned. Your task is an item in a bullet point list (e.g. `- ⌛ Task <NUMBER>: Description`). Under your assigned task there might be a nested bullet point list with additional details. These additional details form part of you task too. Make sure to consider them.
        - You also need to identify all requirements in the `Requirements` section that relate to the task that's been assigned to you. The `Requirements` section describes expected behaviour. You need to implement the code so that it meets the relevant requirements. Try to implement as many requirements as possible.
        - If you have been advised to implement tests, you must also add/update the relevant tests. If you have been advised that no tests are needed, then you must not implement any tests.
        - After you have implemented everything, perform these steps:
          - Review all code that you have added against any coding guidelines that you've been given (for example in `AGENTS.md`). If necessary, refactor the code to meet the guidelines.
          - If the programming languages requires compilation (like Java or TypeScript), compile the application and fix any syntax errors.
          - If you have added or updated tests, or if you've been given instructions of how to run the tests, run the tests and fix any errors.
          - If you have been given instructions of how to run the code formatter and/or linter, run them and fix any errors.
          - As long as any of the steps above require you to make code changes, repeat the cycle of compiling, testing and linting until all steps pass (without requiring any fixes).
        - Advise the agent that called you of the outcome:
          - If there are still unresolved errors (compiler, tests or linting), advise the agent that your assigned task requires more work and has not been fully completed yet.
          - If you have completed your task successfully, advise the agent that your assigned task has been fully implemented.
        ```

    - In addition to these instructions, also advise the subagent which task number it should work on, and whether it should implement tests.

    - Wait until the subagent has finished. Do not run multiple subagents in parallel.

    - If the subagent has confirmed that the task is complete, update `WIP.md` and set the emoji for the task to ✅ (e.g. `- ✅ Task <NUMBER>: Description`). Save file `WIP.md`. Then immediately proceed to the next task on your list of tasks that need implementing. Do not wait for user input or confirmation before proceeding to the next task that needs implementing.

    - If the subagent was aborted, or if it indicated that the task hasn't been fully completed yet, use the "ask user question" tool to ask the user if they either want to retry or skip the task. If the user wants to retry, run the subagent again (for the same task). If the user does not want to retry, immediately proceed to the next task on your list of tasks that need implementing.

5. Implement missed requirements
    - This step only applies if all `Tasks` in `WIP.md` are marked as completed (e.g. `- ✅  Task <NUMBER>: Description`). If any task has an emoji other than ✅ (e.g. ⏸️ or ⌛), skip this step.

    - Perform this step immediately after all tasks have been marked as completed (✅). Do not wait for user input or confirmation. Go ahead with the review immediately (even if all tasks were already marked as completed when you started).

    - Run a subagent to review all requirements and implement those that were previously missed. Do not specify an agent name. Do not use an agent that's specialised in planning or exploration. The agent must be capable of immediately implementing code changes. Provide the subagent with the following instructions (exactly as they are given here, do not modify them):

        ```
        You are a subagent tasked with reviewing an implementation against a set of requirements, and implementing all requirements that were missed and haven't been implemented yet. The requirements are described in a specification document called `WIP.md`. The document contains 3 sections. The `Overview` section contains a brief summary of the feature. The `Requirements` section lists the required functionality and expected behaviours. The `Tasks` section contains the coding tasks that have already been implemented. To complete your mission, follow these steps:
        
        - Read file `WIP.md` from the project's root directory and review its content.
        - Go through each and every bullet point (including the nested bullet point lists) in the `Requirements` section. For each bullet point/requirement do the following:
          - Check in the source code if the requirement has been fully implemented.
          - If the requirement hasn't been fully implemented, make all required code changes to fully implement it.
          - If you have been advised to implement tests, you must also add/update the relevant tests whenever you change the implementation. If you have been advised that no tests are needed, then you must not implement any tests.
        - If you have made any code changes, perform the following steps:
          - Review all code that you have added against all coding guidelines that you've been given (for example through instruction files like `AGENTS.md`). If necessary, refactor the code to meet the guidelines.
          - If the programming languages requires compilation (like Java or TypeScript), compile the application and fix any syntax errors.
          - If you have added or updated tests, or if you've been given instructions of how to run the tests, run the tests and fix any errors.
          - If you have been given instructions of how to run the code formatter and/or linter, run them and fix any errors.
          - As long as any of the steps above require you to make code changes, repeat the cycle of compiling, testing and linting until all steps pass (without requiring any fixes).
          - Do not try to run the application.
        ```

---
