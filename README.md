![hero-image-github-readme-banner](resources/hero-image-github-readme-banner.png)

- [Introduction](#introduction)
- [Commands](#commands)
- [Workflow](#workflow)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage example](#usage)

## Introduction

`WIP.md` is a light-weight spec-driven development framework:

- Simple agent skill (self-contained `SKILL.md`, no scripts, no MCP server, no nothing)
- Tackles one feature at a time (as opposed to running many agents in parallel)
- Asks you clarifying questions for as long as you like (and stops when you tell it to)
- Stores requirements and the implementation plan in a single specification (called `WIP.md`)
- You'll know the implementation plan upfront and have a say in what's implemented where
- You can discuss requirements and the implementation plan at the same time (no waterfall model)
- The whole specification is implemented in one go (running subagents sequentially)
- Work can be resumed in a new session with a clean context at any time

## Commands

`WIP.md` provides the following commands:

- `/wip-specify` - create/update/refine the specification
- `/wip-implement` - implement the tasks

All commands are implemented as agent skills. However, they must be invoked explicitly via slash commands. Agents won't use the skills automatically.

## Workflow

1. You describe the feature and requirements (and provide technical direction if you want to)
2. AI asks you clarifying questions (until you tell it to stop)
3. AI creates a specification (collating the requirements and devising an implementation plan)
4. Optional: You ask AI to make changes to the specification (or change it yourself manually)
5. Optional: AI can ask you more clarifying questions (if you ask it to)
6. AI implements the specification (runs subagents sequentially until all tasks are completed)
7. You delete the specification (or archive it, if you want to keep it)

## Installation

Either download the skills from the [skills](skills) folder, or install them via:

```sh
npx -y skills add david-04/wip-md
```

You need to install at least `wip-specify` and `wip-implement`.

## Configuration

After the agent has implemented a task, it can run command-line tools to:

- Compile the application
- Format and/or lint the code
- Run the tests

To enable any of these steps, provide the respective commands in `AGENTS.md`.

## Tests

`WIP.md` does not support test-driven development (where tests are implemented first). But it does support tests in general. It is not necessary to mention tests in the specification. If the project uses tests, the agent will automatically add and update them when making code changes.

To allow the agent to detect if tests are required, provide instructions in `AGENTS.md`. If the project uses tests, specify the command to run them. Otherwise, state in `AGENTS.md` that the project does not use tests.

## Usage example

Start by explicitly invoking the `/wip-specify` skill and describing the feature that you want to build:

> `/wip-specify` Create a command-line parser. It should support positional arguments, flags (`--flag`), and options with values (`--key=value`). Initially, the following parameters should be supported:
>
> - `--help` Display usage instructions with a list of options
> - `--output=[FILE]` Specify the output file to generate
>
> Positional arguments and option values must be trimmed. The trimmed values must not be empty. Treat unknown options as an error.

AI will ask you 3 clarifying questions like this:

> Support short flags like -h and -v?
> [1] Yes
> [2] No
> [3] Enter custom answer: ________________________________

After each round of questions, AI will ask you if you want to continue with more questions:

> Ask more questions?
> [1] Yes
> [2] No

After you've told it to stop asking questions, AI will use a subagent to prepare an implementation plan. After that, it will document all requirements and the implementation plan in the specification file `WIP.md` in the project's root directory:

> **Overview**
>
> Add a command-line parser that recognises positional arguments, boolean flags, and options with values. It supports `--help` and `--output=[FILE]`, trims values, and rejects unknown options with an error. The parser validates inputs and provides usage text when requested.
>
> **Requirements**
>
> - Parsing behavior
>   - Support positional arguments and preserve their order
>   - Support boolean flags using the long form `--flag`
>   - Support options with values using the form `--key=value`
>   - Trim whitespace from positional arguments and option values
>   - Treat trimmed values as invalid if they are empty
> - Arguments and options
>   - Provide `--help` to display usage instructions with a list of options
>   - Provide `--output=[FILE]` to specify the output file to generate
>   - `--output` must use the `--output=FILE` form; do not accept `--output FILE`
>   - Do not implement short aliases like `-h` or `-o` (no short flags)
> - Validation and errors
>   - Unknown options must be treated as an error and cause the parser to exit with an error message
>   - Option names and values must be trimmed before validation
>   - If a required value is empty after trimming, treat this as an error
> - Implementation instructions
>   - Implement the parser as a synchronous module appropriate for a Node.js TypeScript CLI
>
> **Tasks**
>
> - ⏸️ Task 1: Create `src/cliParser.ts` and add `parseArgs` and `ParsedArgs`
>   - Create a new synchronous module at `src/cliParser.ts`
>   - Add and export function `parseArgs(args: readonly string[]): ParsedArgs`
>   - Add and export type `ParsedArgs`
>
> - ⏸️ Task 2: Implement positional, boolean flags, and `--key=value` options in `src/cliParser.ts`
>   - Update `src/cliParser.ts` to parse positional arguments while preserving order
>   - Parse boolean flags using the long form `--flag`
>   - Parse options with values using the form `--key=value`
>
> - ⏸️ Task 3: Implement trimming, empty-value validation, and unknown-option rejection in `src/cliParser.ts`
>   - Ensure `parseArgs` trims whitespace from positional arguments and option values
>   - Validate that trimmed values are not empty; return or throw an error on invalid values
>   - Treat unknown options as an error and ensure `parseArgs` surfaces this to callers
>
> - ⏸️ Task 4: Add support for `--help` and `--output=FILE` handling
>   - Update `src/cliParser.ts` to recognise `--help` and `--output=FILE` and include them in `ParsedArgs`
>   - Ensure `--output` only accepts `--output=FILE` form (do not accept space-separated values)
>
> - ⏸️ Task 5: Integrate parser into the CLI entry point `src/directoroo.ts`
>   - Update `src/directoroo.ts` to import and use `parseArgs(process.argv.slice(2))`
>   - Display usage text when `--help` is present
>   - Display errors and exit appropriately when `parseArgs` reports invalid input or unknown options

After reviewing the specification, you can make changes to the file yourself, or ask AI to make changes, for example:

> - All code related to command-line parsing should go into src/utils/cli.ts
> - Remove task 5
> - ...

You can also ask AI to ask you more clarifying questions:

> Ask me questions

To resume refinement with a fresh context window, start a new chat session and explicitly invoke skill `/wip-specify`:

> /wip-specify Add another option `--version` which displays the program's current version number

Once you're happy with the specification and ready to implement the feature, start a new chat session (to purge the context), and invoke skill `/wip-implement`:

> /wip-implement

To implement only some tasks, provide the relevant instructions when invoking the skill:

> /wip-implement Implement tasks 1, 2, and 3
