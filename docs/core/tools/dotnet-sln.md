---
title: dotnet sln command
description: The dotnet-sln command provides a convenient option to add, remove, and list projects in a solution file.
ms.date: 02/24/2025
---
# dotnet sln

**This article applies to:** ✔️ .NET Core 3.1 SDK and later versions

## Name

`dotnet sln` - Lists or modifies the projects in a .NET solution file, or migrates the file to an *.slnx* file.

## Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] [command]

dotnet sln [command] -h|--help
```

## Description

The `dotnet sln` command provides a convenient way to list and modify projects in a solution file.

### Create a solution file

To use the `dotnet sln` command, the solution file must already exist. If you need to create one, use the [dotnet new](dotnet-new.md) command with the `sln` template name.

The following example creates a *.sln* file in the current folder, with the same name as the folder:

```dotnetcli
dotnet new sln
```

The following example creates a *.sln* file in the current folder, with the specified file name:

```dotnetcli
dotnet new sln --name MySolution
```

The following example creates a *.sln* file in the specified folder, with the same name as the folder:

```dotnetcli
dotnet new sln --output MySolution
```

## Arguments

- **`SOLUTION_FILE`**

  The solution file to use (either an *.sln* or *.slnx* file).

  If unspecified, the command searches the current directory for an *.sln* or *.slnx* file and, if it finds exactly one, uses that file. If multiple solution files are found, the user is prompted to specify a file explicitly. If none are found, the command fails.

## Options

[!INCLUDE [help](../../../includes/cli-help.md)]

## Commands

The following commands are available:

- [`list`](#list)
- [`add`](#add)
- [`remove`](#remove)
- [`migrate`](#migrate)

### `list`

Lists all projects in a solution file.

#### Synopsis

```dotnetcli
dotnet sln list [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The solution file to use (either an *.sln* or *.slnx* file).

  If unspecified, the command searches the current directory for an *.sln* or *.slnx* file and, if it finds exactly one, uses that file. If multiple solution files are found, the user is prompted to specify a file explicitly. If none are found, the command fails.

#### Options

[!INCLUDE [help](../../../includes/cli-help.md)]

### `add`

Adds one or more projects to the solution file.

#### Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] add [--in-root] [-s|--solution-folder <PATH>] <PROJECT_PATH> [<PROJECT_PATH>...]
dotnet sln add [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The solution file to use (either an *.sln* or *.slnx* file).

  If unspecified, the command searches the current directory for an *.sln* or *.slnx* file and, if it finds exactly one, uses that file. If multiple solution files are found, the user is prompted to specify a file explicitly. If none are found, the command fails.

- **`PROJECT_PATH`**

  The path to the project or projects to add to the solution. Unix/Linux shell [globbing pattern](https://en.wikipedia.org/wiki/Glob_(programming)) expansions are processed correctly by the `dotnet sln` command.

  If `PROJECT_PATH` includes folders that contain the project folder, that portion of the path is used to create [solution folders](/visualstudio/ide/solutions-and-projects-in-visual-studio#solution-folder). For example, the following commands create a solution with `myapp` in solution folder `folder1/folder2`:

  ```dotnetcli
  dotnet new sln
  dotnet new console --output folder1/folder2/myapp
  dotnet sln add folder1/folder2/myapp
  ```

  You can override this default behavior by using the `--in-root` or the `-s|--solution-folder <PATH>` option.

#### Options

[!INCLUDE [help](../../../includes/cli-help.md)]

- **`--in-root`**

  Places the projects in the root of the solution, rather than creating a [solution folder](/visualstudio/ide/solutions-and-projects-in-visual-studio#solution-folder). Can't be used with `-s|--solution-folder`.

- **`-s|--solution-folder <PATH>`**

  The destination [solution folder](/visualstudio/ide/solutions-and-projects-in-visual-studio#solution-folder) path to add the projects to. Can't be used with `--in-root`.

### `remove`

Removes a project or multiple projects from the solution file.

#### Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] remove <PROJECT_PATH> [<PROJECT_PATH>...]
dotnet sln [<SOLUTION_FILE>] remove [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The solution file to use (either an *.sln* or *.slnx* file).

  If unspecified, the command searches the current directory for an *.sln* or *.slnx* file and, if it finds exactly one, uses that file. If multiple solution files are found, the user is prompted to specify a file explicitly. If none are found, the command fails.

- **`PROJECT_PATH`**

  The path to the project or projects to remove from the solution. Unix/Linux shell [globbing pattern](https://en.wikipedia.org/wiki/Glob_(programming)) expansions are processed correctly by the `dotnet sln` command.

#### Options

[!INCLUDE [help](../../../includes/cli-help.md)]

### `migrate`

Generates an *.slnx* solution file from an *.sln* file.

#### Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] migrate
dotnet sln [<SOLUTION_FILE>] migrate [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The *.sln* solution file to migrate.

  If unspecified, the command searches the current directory for an *.sln* file and, if it finds exactly one, uses that file. If multiple *.sln* files are found, the user is prompted to specify a file explicitly. If none are found, the command fails.

  If you specify an *.slnx* file instead of an *.sln* file, or if an *.slnx* file with the same file name (minus the *.sln* extension) already exists in the directory, the command fails.

#### Options

[!INCLUDE [help](../../../includes/cli-help.md)]

## Examples

- List the projects in a solution:

  ```dotnetcli
  dotnet sln todo.slnx list
  ```

- Add a C# project to a solution:

  ```dotnetcli
  dotnet sln add todo-app/todo-app.csproj
  ```

- Remove a C# project from a solution:

  ```dotnetcli
  dotnet sln remove todo-app/todo-app.csproj
  ```

- Add multiple C# projects to the root of a solution:

  ```dotnetcli
  dotnet sln todo.slnx add todo-app/todo-app.csproj back-end/back-end.csproj --in-root
  ```

- Add multiple C# projects to a solution:

  ```dotnetcli
  dotnet sln todo.slnx add todo-app/todo-app.csproj back-end/back-end.csproj
  ```

- Remove multiple C# projects from a solution:

  ```dotnetcli
  dotnet sln todo.slnx remove todo-app/todo-app.csproj back-end/back-end.csproj
  ```

- Add multiple C# projects to a solution using a globbing pattern (Unix/Linux only):

  ```dotnetcli
  dotnet sln todo.slnx add **/*.csproj
  ```

- Add multiple C# projects to a solution using a globbing pattern (Windows PowerShell only):

  ```dotnetcli
  dotnet sln todo.slnx add (ls -r **/*.csproj)
  ```

- Remove multiple C# projects from a solution using a globbing pattern (Unix/Linux only):

  ```dotnetcli
  dotnet sln todo.slnx remove **/*.csproj
  ```

- Remove multiple C# projects from a solution using a globbing pattern (Windows PowerShell only):

  ```dotnetcli
  dotnet sln todo.slnx remove (ls -r **/*.csproj)
  ```

- Generate an *.slnx* file from a *.sln* file:

  ```dotnetcli
  dotnet sln todo.sln migrate
  ```

- Create a solution, a console app, and two class libraries. Add the projects to the solution, and use the `--solution-folder` option of `dotnet sln` to organize the class libraries into a solution folder.

  ```dotnetcli
  dotnet new sln -n mysolution
  dotnet new console -o myapp
  dotnet new classlib -o mylib1
  dotnet new classlib -o mylib2
  dotnet sln mysolution.slnx add myapp\myapp.csproj
  dotnet sln mysolution.slnx add mylib1\mylib1.csproj --solution-folder mylibs
  dotnet sln mysolution.slnx add mylib2\mylib2.csproj --solution-folder mylibs
  ```

  The following screenshot shows the result in Visual Studio 2019 **Solution Explorer**:

  :::image type="content" source="media/dotnet-sln/dotnet-sln-solution-folder.png" alt-text="Solution Explorer showing class library projects grouped into a solution folder.":::

## See also

- [dotnet/sdk GitHub repo](https://github.com/dotnet/sdk) (.NET CLI source)
