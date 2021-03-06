# DotNetCobble
A dotnet tool for cobbling together a solution file from a set of target projects that includes all of their project dependencies.

### Description
Sometimes we find ourselves working in a solution that has many projects and complicated dependency graphs when we could get better performance out of Visual Studio and third party plugins if the solution were much smaller. However, Visual Studio has the curious quirk that, while `dotnet build` is fully capable of building a project along with its dependencies, Visual Studio will fail if any project dependency isn't present in the currently loaded solution. Typically, crafting a one-off solution for a particular project to ease these pain points is time-consuming as you must first determine your entire project dependency graph for the target project (or set of projects) and then add them one-at-a-time to the solution.

DotNetCobble simplifies that by providing simple command-line tooling for creating a solution file from a target project (or folder of projects) that contains the target project(s) as well as all of their project dependencies.

This is particularly useful in an microservices environment where core foundational projects are used uniformly across many domains, allowing you to keep (or create) one large solution to perform large-scale refactorings or, if you already have that, create smaller solution files for more nimble development.

## To Install
`dotnet tool install -g DotNetCobble`

## To Uninstall
`dotnet tool uninstall -g DotNetCobble`

## Usage
### Creating a Solution From a Single Project
`dotnet cobble project -p C:\Path\To\Project.csproj`

In this case, DotNetCobble will do the following:
* Create a solution named `MagicSolution.sln` in the current directory.
* Add the target project to the solution, at the root of the solution.
* Recursively scan the dependencies of each project the target project depends on.
* Add all of the target project's dependencies to the solution under a solution folder named `Dependencies`.

##### Creating a Solution From Multiple Projects
`dotnet cobble folder -t C:\Path\To\SomeFolder`

In this case, DotNetCobble will do the following:
* Create a solution named `MagicSolution.sln` in the current directory.
* Scan the target folder for `.csproj` files, including nested directories.
* Add all of the located `.csproj` files the solution, at the root of the solution.
* Recursively scan the dependencies of each project the target projects depends on.
* Add all of the target projects' dependencies to the solution under a solution folder named `Dependencies`.

##### Help
Executing `dotnet cobble`, `dotnet cobble project` or `dotnet cobble folder` with no arguments will provide additional help.

## Limitations
This first release has several limitations, among them:
* It can only operate on `.csproj` files.
* You cannot control the filename of the solution file that gets created (you can rename it afterwards, though).
* You cannot control the behavior around solution folders or manipulate the solution structure as part of the tool.