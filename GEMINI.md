# OrcaSlicer Project Context

## Critical ! important
Would you be able please to tag any changes made to the code with the //ORCA: comment tag and providing a very brief 1-2 line explanation of the change made? Also ideally commenting out instead of deleting the previous code (if applicable).

## Project Overview
OrcaSlicer is an open-source, next-generation 3D slicer forked from Bambu Studio (which itself is based on PrusaSlicer/Slic3r). It is built primarily in **C++** (C++17 standard with some C++20 features) and uses **wxWidgets** for the cross-platform GUI. The build system is **CMake**.

## Architecture
The codebase is modular, separating core slicing logic from the user interface and platform specifics.

### Key Components
*   **`src/libslic3r/`**: The core slicing engine. Contains geometry processing, G-code generation, and algorithms for infill, support, and wall generation. Platform-independent.
*   **`src/slic3r/GUI/`**: The graphical user interface, built with wxWidgets. Handles user interaction, rendering, and application logic.
*   **`src/OrcaSlicer.cpp`**: The main entry point for the application.
*   **`deps/`**: External dependencies (Boost, TBB, OpenVDB, etc.). The build process handles these separately.
*   **`resources/`**: runtime resources including:
    *   `profiles/`: Printer, filament, and process profiles.
    *   `i18n/`: Localization files.
    *   `images/` & `icons/`: Visual assets.

## Building and Running

### Windows
*   **Build All:** `build_release_vs2022.bat`
*   **Debug Build:** `build_release_vs2022.bat debug`
*   **Build Dependencies Only:** `build_release_vs2022.bat deps`
*   **Build Slicer Only:** `build_release_vs2022.bat slicer` (Use this after dependencies are built for faster iteration)

### macOS
*   **Build All:** `./build_release_macos.sh`
*   **Build Dependencies Only:** `./build_release_macos.sh -d`
*   **Build Slicer Only:** `./build_release_macos.sh -s`

### Linux
*   **First-time Setup:** `./build_linux.sh -u` (Installs system deps)
*   **Build All:** `./build_linux.sh -dsi`
*   **Build Slicer Only:** `./build_linux.sh -s`

### Build System Details
*   **CMake Version:** Minimum 3.13.
*   **Output Directory:** Builds typically go to `build/`. Dependencies go to `deps/build/`.
*   **Generators:** Visual Studio (Windows), Xcode/Ninja (macOS), Ninja (Linux).

## Testing
Tests are located in the `tests/` directory and use the **Catch2** framework.

*   **Run All Tests:**
    ```bash
    cd build
    ctest
    ```
*   **Run Specific Test Suite:**
    ```bash
    ctest --test-dir ./tests/libslic3r/libslic3r_tests
    ```
*   **Test Structure:**
    *   `tests/libslic3r/`: Core geometry and algorithm tests.
    *   `tests/fff_print/`: FFF specific slicing and G-code tests.
    *   `tests/sla_print/`: SLA specific tests.

## Development Conventions
*   **Language:** C++17 is the baseline.
*   **Style:**
    *   **Classes:** PascalCase (e.g., `PrintObject`).
    *   **Functions/Variables:** snake_case (e.g., `generate_gcode`).
    *   **Headers:** Use `#pragma once`.
*   **Memory Management:** Heavy use of smart pointers and RAII.
*   **Threading:** Uses Intel TBB for parallelization. Be careful with shared state in the slicing core.
*   **Internationalization:** All user-facing strings must be wrapped for localization (see `localization/i18n/`).

## Key Files for Navigation
*   `CMakeLists.txt`: Root build configuration.
*   `src/CMakeLists.txt`: Main source build configuration.
*   `src/libslic3r/Print.cpp`: High-level orchestration of the slicing process.
*   `src/libslic3r/PrintConfig.cpp`: Definitions for all print settings (useful when adding new parameters).

## Context Preservation Workflow
To avoid frequent commits (which trigger long rebuilds) while preventing context loss between chat sessions:
1.  **current_task.md**: Before a context clear or at logical stopping points, write the current state, recent changes, and next steps to `current_task.md` in the root directory.
2.  **Restoration**: At the start of a new session, read `current_task.md` and run `git diff`.
3.  **Cleanup**: When finally committing changes via git, clear or delete `current_task.md`.