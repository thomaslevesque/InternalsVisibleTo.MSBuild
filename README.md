# InternalsVisibleTo.MSBuild

[![NuGet version](https://img.shields.io/nuget/v/InternalsVisibleTo.MSBuild.svg?logo=nuget)](https://www.nuget.org/packages/InternalsVisibleTo.MSBuild)

Enables declaring `InternalsVisibleTo` items in a .NET project file, rather than declaring them in an AssemblyInfo.cs file.

**IMPORTANT: starting with .NET 5.0, the SDK [already provides support for this](https://github.com/dotnet/sdk/pull/3439). You don't need this package if you're using the .NET 5.0 SDK or later.**

## How to use

1. Install the `InternalsVisibleTo.MSBuild` NuGet package.
2. Edit your csproj file and add `<InternalsVisibleTo>` items in your project for each assembly that should have access
to the internals of the current project:

    ```xml
      <ItemGroup>
        <InternalsVisibleTo Include="$(AssemblyName).UnitTests" />
        <InternalsVisibleTo Include="SomeOtherAssembly" />
        <InternalsVisibleTo Include="StronglyNamedAssembly, PublicKey=0123....." />
      </ItemGroup>
    ```
    
    This will generate the appropriate `InternalsVisibleTo` attributes for your assembly.
    
    Alternatively, you can also use `<InternalsVisibleToSuffix>` to specify that assemblies named `$(AssemblyName).<suffix>` will have access to the internals of the current project:
    
    ```xml
      <ItemGroup>
        <!-- $(AssemblyName).UnitTests will have access to the internals of this project -->
        <InternalsVisibleToSuffix Include=".UnitTests" />
      </ItemGroup>
    ```

## Note

In fact, it's already possible to declare `InternalsVisibleTo` attributes in the project file without this package, but the syntax is ugly and hard to remember:

```xml
  <ItemGroup>
    <AssemblyAttribute Include="System.Runtime.CompilerServices.InternalsVisibleTo">
      <_Parameter1>SomeOtherAssembly</_Parameter1>
    </AssemblyAttribute>
  </ItemGroup>
```

This package just makes things easier by transforming `<InternalsVisibleTo>` elements into appropriate `<AssemblyAttribute>` elements.
