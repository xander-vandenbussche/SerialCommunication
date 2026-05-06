# Copilot instructions for SerialCommunication

This file helps future Copilot sessions understand how to build, run, and reason about this repository.

---

## Quick build/run commands

- Open the solution in Visual Studio: open SerialCommunication.slnx (double-click the .slnx in Explorer or use Visual Studio "Open -> Project/Solution").
- Build (CLI, Windows):
  - msbuild "SerialCommunication.slnx" /p:Configuration=Debug
  - msbuild "SerialCommunication\SerialCommunication.csproj" /p:Configuration=Debug
- Run the compiled executable:
  - .\SerialCommunication\bin\Debug\SerialCommunication.exe
- Note: project targets .NET Framework 4.7.2; Visual Studio (2017/2019/2022) or msbuild that supports that framework is required.

No test runner or linter is present in this repository.


## How to run a single scenario / debug

- Run under the debugger from Visual Studio (F5) and interact with the UI. There are no unit/integration tests to run.
- To exercise serial-port enumeration: open the app and open the Poort combo-box (it refreshes port list on DropDown).
- Build+Run a single build configuration via msbuild by changing /p:Configuration=Debug/Release as needed.


## High-level architecture (big picture)

- Single Windows Forms application (WinForms) written in C# targeting .NET Framework 4.7.2.
- Entry point: Program.Main -> Application.Run(new Form1()).
- UI surface: Form1 (Form1.cs + Form1.Designer.cs). The Designer contains the form layout and control wiring.
- Serial I/O: System.IO.Ports is referenced; the UI contains controls for port selection and connection settings (baud rate, parity, stop bits, handshake, DTR/RTS). The DropDown handler for comboBoxPoort refreshes available COM ports.
- Resources: images are embedded via the project Resources (see Resources folder entries in the .csproj).
- Project file: SerialCommunication\SerialCommunication.csproj contains standard WinForms project configuration (output paths, references, target framework).


## Key repository-specific conventions

- Naming: UI controls use Hungarian-like prefixes (comboBoxPoort, buttonConnect, labelPoort, trackBarPWM9, etc.). Event handlers follow the pattern control_event (e.g., buttonConnect_Click, cboPoort_DropDown).
- Language: UI text and identifiers use Dutch. Expect labels, tab titles, and comments in Dutch ("Poort", "Instellingen", "Oefening").
- Designer-first workflow: UI layout and control declarations live in Form1.Designer.cs (auto-generated). Code-behind (event handlers and logic) live in Form1.cs. Keep manual logic out of Designer file.
- Serial port enumeration: comboBoxPoort.DropDown repopulates with SerialPort.GetPortNames().Distinct() — use that event to refresh available ports rather than scanning elsewhere.
- Connection logic location: buttonConnect_Click in Form1.cs is the intended place for connection/disconnection logic (currently minimal / placeholder).
- Embedded resources: images are included as None/EmbeddedResource entries in the .csproj and available via Properties.Resources when referenced.
- Project layout: single-project solution; expect output under SerialCommunication\bin\{Configuration}. No external packages (no NuGet package references present in the csproj).


## Where to look for common tasks

- Add serial protocol parsing/serialization: Form1.cs and new helper classes in project root.
- Add unit tests: create a new test project (recommended: MSTest / NUnit / xUnit targeting .NET Framework or .NET Core compatibility) and add to solution.
- Add CI/linters: no CI configured; if adding, include msbuild steps or dotnet (if migrating to SDK-style) in the workflow.


## Files and locations referenced by Copilot

- Entry / run: SerialCommunication\Program.cs
- Main UI and logic: SerialCommunication\Form1.cs and SerialCommunication\Form1.Designer.cs
- Project config: SerialCommunication\SerialCommunication.csproj
- Resources: SerialCommunication\Resources\* and Properties\Resources.resx


---

If you want this file to include example prompts for common tasks (e.g., "implement serial connect/disconnect flow", "add logging to serial I/O", "migrate project to SDK-style csproj / .NET 6+"), say which tasks to include and Copilot will append short ready-to-use prompts.
