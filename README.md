# project 1 Game (Stone, Paper, Scissors)

![Language](https://img.shields.io/badge/language-C%2B%2B-00599C)
![Build System](https://img.shields.io/badge/build-Visual%20Studio%20(MSBuild)-5C2D91)
![Platform](https://img.shields.io/badge/platform-Windows-0078D6)
![License](https://img.shields.io/badge/license-Not%20specified-lightgrey)

A console-based, multi-round Stone/Paper/Scissors game where a human player competes against the computer.

## Overview

`project 1 Game` is a C++ console application implementing the classic Stone-Paper-Scissors game. The player chooses how many rounds to play (1–10), then plays each round against a computer opponent that picks randomly. The program tracks per-round and overall results, prints a colored, formatted round-by-round and final summary, and lets the player choose to play again.

## Features

- **Configurable match length**: the player is prompted to choose between 1 and 10 rounds per game, with input re-prompted until a valid value is entered.
- **Player vs. Computer gameplay**: the player selects Stone, Paper, or Scissors each round via numeric input (1/2/3); the computer's choice is generated randomly.
- **Round-winner logic**: standard Stone/Paper/Scissors rules (Stone beats Scissors, Scissors beats Paper, Paper beats Stone) determine the round winner, with draws handled when both choices match.
- **Per-round result display**: after each round, the round number, both players' choices, and the round winner are printed to the console.
- **Color-coded console feedback**: the console background/foreground color changes based on the round/game winner — green for a Player1 win, red (with an audible beep) for a Computer win, and yellow for a draw — via Windows `system("color ...")` calls.
- **Overall game results summary**: after all rounds are played, the program displays total rounds played, how many times Player1 won, how many times the Computer won, how many draws occurred, and the final overall winner (determined by whoever has the most round wins; a tie in win counts is a draw).
- **Replay loop**: after a game finishes, the player is asked whether they want to play again (Y/N), and the game restarts (clearing the screen and resetting the console color) if they choose to continue.

## Screenshots

_No screenshots are included in the provided project files. This is a text-based console application._

## Demo

Not specified.

## Tech Stack

| Category | Technology |
|---|---|
| Language | C++ |
| Standard Library | `<iostream>`, `<cstdlib>` |
| Build System / IDE | Visual Studio 2022 (Solution/Project files: `.sln`, `.vcxproj`) |
| Toolset | MSVC `v143` platform toolset |
| Target Platforms | Win32 and x64 (Debug and Release configurations) |
| Application Type | Windows Console Application |

## Architecture

The application is a single-file, procedural C++ console program (`project 1 Game.cpp`) with no classes — game state is modeled using two plain structs and behavior is implemented as a set of free functions.

**Data structures:**
- `enGameChoice` — enum representing a Stone/Paper/Scissors choice (`Stone = 1`, `Paper = 2`, `Scissors = 3`).
- `enWinner` — enum representing a round or game winner (`Player1 = 1`, `Computer = 2`, `Draw = 3`).
- `stRoundInfo` — struct holding a single round's number, both players' choices, the round winner, and the winner's display name.
- `stGameResults` — struct holding the total number of rounds played, win counts for each side, the draw count, the overall game winner, and the winner's display name.

**Core functions:**
- `RandomNumber(From, To)` — generates a random integer in an inclusive range, used to produce the computer's move.
- `WhoWonTheRound(RoundInfo)` — applies Stone/Paper/Scissors rules to determine the round winner from both players' choices.
- `WhoWonTheGame(Player1WinTimes, ComputerWinTimes)` — compares total win counts to determine the overall game winner (or a draw if tied).
- `ReadPlayer1Choice()` / `ReadHowManyRounds()` — validate and loop on console input until the player enters a value in the accepted range.
- `GetComputerChoice()` — wraps `RandomNumber` to produce the computer's move.
- `PlayGame(HowManyRounds)` — the main round loop: reads choices, determines the round winner, updates counters, and prints round results; returns a filled `stGameResults`.
- `PrintRoundResults` / `ShowGameOverScreen` / `ShowFinalGameResults` — console output/formatting functions, including the `Tabs()` helper used for indentation.
- `SetWinnerScreenColor(Winner)` — changes the console color scheme (and beeps on a computer win) using the Windows-specific `system("color ...")` command.
- `ResetScreen()` — clears the console and resets its color using `system("cls")` / `system("color 0F")`.
- `StartGame()` — top-level game loop: runs a full game, shows results, and asks the player whether to play again.
- `main()` — seeds the random number generator with the current time and starts the game via `StartGame()`.

### Notable implementation details

- Random number generation is seeded once in `main()` via `srand((unsigned)time(NULL))`, though `<ctime>` is not explicitly included (the program compiles because `time` is transitively available via other included headers on the target toolchain).
- Console color changes and screen clearing rely on Windows-specific `system("color ...")` / `system("cls")` calls, making the program Windows-only as written.
- The `stGameResults` struct field for the computer's win count is named `Computer2WinTimes` (rather than `ComputerWinTimes`), an inconsistency with the rest of the naming in the codebase.
- The `Tabs()` helper function both builds and directly prints indentation via `cout` as a side effect while also returning the tab string, and is called in a way where its return value is discarded in the `ShowGameOverScreen`/`ShowFinalGameResults` output helpers.

## Project Structure

```
project 1 Game/
├── project 1 Game.sln       # Visual Studio solution file
├── project 1 Game.vcxproj   # MSBuild/Visual Studio C++ project file
└── project 1 Game.cpp       # Single-file source: all game logic and console I/O
```

> **Note:** Only the solution, project, and single `.cpp` source file were provided. Standard companion files for a Visual Studio C++ project (e.g., `.vcxproj.filters`, `.vcxproj.user`, `pch.h`/`pch.cpp` if precompiled headers are used) were not included and are not referenced as required by the provided `.vcxproj`.

## Requirements

- Windows OS (the program uses Windows-specific `system("color ...")` and `system("cls")` calls).
- Visual Studio 2022 (or compatible), with the **Desktop development with C++** workload, since the project targets the `v143` platform toolset.
- No external libraries or third-party dependencies — only the C++ standard library (`<iostream>`, `<cstdlib>`) is used.

## Installation

1. Clone or download the repository.
2. Open `project 1 Game.sln` in Visual Studio 2022.
3. Select a configuration/platform (`Debug` or `Release`; `Win32` or `x64`).
4. Build the solution (**Build → Build Solution**, or `Ctrl+Shift+B`).

Alternatively, from a **Developer Command Prompt for VS 2022**, you can build via MSBuild:

```
msbuild "project 1 Game.sln" /p:Configuration=Release /p:Platform=x64
```

## Configuration

Not specified. No configuration files, environment variables, or command-line arguments are used — the game is fully interactive via console prompts (number of rounds, choice per round, and replay Y/N).

## Running the Project

Run the built executable (e.g., `project 1 Game.exe` from the `x64\Release` or `x64\Debug` output folder) from a console/terminal, or press `F5`/`Ctrl+F5` in Visual Studio to build and run directly.

Once running:
1. Enter how many rounds to play (1–10).
2. For each round, enter your choice: `1` for Stone, `2` for Paper, `3` for Scissors.
3. View the round result and, at the end, the final game summary.
4. Enter `Y` to play again or any other value to exit the loop.

## Development

Open `project 1 Game.sln` in Visual Studio and edit `project 1 Game.cpp` directly; there is a single source file with no additional modules.

## Build

Build via Visual Studio (**Build Solution**) or MSBuild as shown in [Installation](#installation). Four build configurations are defined: `Debug|Win32`, `Debug|x64`, `Release|Win32`, and `Release|x64`.

## Testing

Not specified. No test project or testing framework is referenced in the provided files.

## API Documentation

Not applicable. This is a standalone console application with no exposed APIs or network endpoints.

## Database

Not applicable. No database or persistent storage is used — all game state exists only in memory for the duration of a run.

## Deployment

Not specified. No Docker, CI/CD, or packaging/installer configuration was provided. The application is deployed by distributing the compiled `.exe` produced by the Visual Studio build.

## Security Notes

Not applicable in the traditional sense. The application only reads numeric input from the console (with basic range validation via input-loop retries) and makes local `system()` calls to change console color/clear the screen — no network communication, authentication, or external/untrusted data is involved.

## Performance Notes

Not applicable. This is a lightweight, turn-based console application with no performance-sensitive operations, caching, or background processing.

## Known Limitations

- Windows-only: the game relies on `system("color ...")` and `system("cls")`, which are not portable to other operating systems.
- `<ctime>` is not explicitly included even though `time(NULL)` is used to seed the random number generator in `main()`.
- The `stGameResults.Computer2WinTimes` field name is inconsistent with the rest of the codebase's naming conventions.
- Input validation only checks that the round count and choice are within valid numeric ranges; non-numeric input is not explicitly handled and could cause unexpected behavior with `cin`.
- No automated tests are present.
- The game only supports a single human player against the computer; there is no player-vs-player mode.

## Future Improvements

- Replace Windows-specific `system("color ...")`/`system("cls")` calls with a cross-platform console library to support non-Windows builds.
- Add `<ctime>` explicitly for clarity and portability.
- Rename `Computer2WinTimes` to `ComputerWinTimes` for consistency.
- Add robust handling of non-numeric console input.
- Add unit tests for the round/game winner-determination logic.
- Consider extracting game logic into reusable functions/classes separate from console I/O to improve testability.

## Contributing

Contributions are welcome. To contribute:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/my-feature`).
3. Make your changes and commit them with clear messages.
4. Push to your fork and open a Pull Request describing your changes.

Please keep changes consistent with the existing procedural C++ style used in this project.

## License

Not specified. No license file was included in the provided project files.

## Author

Baraa Mardini

