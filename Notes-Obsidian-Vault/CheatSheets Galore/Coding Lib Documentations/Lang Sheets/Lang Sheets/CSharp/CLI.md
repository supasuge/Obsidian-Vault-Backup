# .NET CLI
- `dotnet new` - Creates a new .NET project. you then can specify the type of project (`console, classlib, webapi, mvc,` etc.) Ex: `dotnet new console` will create a new console application.
- `dotnet build` - Builds a .NET project and all it's dependencies. The `-c` or `--configuration` option can be used to specify the build configuration `debug` or `release`
- `dotnet run`: Builds and runs the .NET project. It is typically used during the development process to run the application for testing or debugging purposes.
- `dotnet test`: Runs unit tests in a .NET project using a test framework such as `MSTest`, `NUnit`, or `xUnit`.
- `dotnet publish`: Packs the application and its dependencies into a folder for deployment to a hosting system. The `-r` or `--runtime` option can be used to specify the target runtime.
- `dotnet add package`: Adds a NuGet package reference to the project file. You specify the package by name. For example, `dotnet add package Newtonsoft.Json`.
- `dotnet remove package`: Removes a NuGet package reference from the project file. Similar to the `add package` command, you specify the package to remove by name.
- `dotnet restore`: Restores the dependencies and tools of a project. This command is implicitly run when you run `dotnet new`, `dotnet build`, `dotnet run`, `dotnet test`, `dotnet publish`, and `dotnet pack`.
- `dotnet clean`: Cleans the output of a project. This command is typically used before you build the project again, as it deletes all the previously compiled files, ensuring that you start from a clean state.
- `dotnet --info`: Displays detailed information about the installed .NET environment, including installed versions and all runtime environments.

