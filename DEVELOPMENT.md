# SPC42 .NET Project Templates

## Adding New Templates

Templates are managed as git submodules in the `/content` directory. This allows you to reference external project repositories while keeping them synchronized with their source.

### Adding a Template via Git Submodule

1. Navigate to the repository root:
   ```bash
   cd /path/to/DotNetProjectTemplates
   ```

2. Add the template repository as a submodule in the `content` directory:
   ```bash
   git submodule add <repository-url> content/<TemplateName>
   ```

   Example:
   ```bash
   git submodule add https://github.com/example/MyTemplate content/MyTemplate
   ```

### Updating Submodule Templates

To update a template to the latest version from its source repository:

```bash
cd content/<TemplateName>
git pull origin main
cd ../..
git add content/<TemplateName>
git commit -m "Update <TemplateName> template"
```

### Cloning This Repository with Submodules

When cloning this repository, use the `--recursive` flag to automatically clone all submodules:

```bash
git clone --recursive https://github.com/SPC42/DotNetProjectTemplates.git
```

If you've already cloned the repository without submodules, initialize them with:

```bash
git submodule update --init --recursive
```

## Testing Templates Locally

Before publishing your templates, you should test them locally to ensure they work as expected.

### Installing Templates Locally

1. Navigate to the repository root:
   ```bash
   cd /path/to/DotNetProjectTemplates
   ```

2. Install the templates from the local project:
   ```bash
   dotnet new install .
   ```

   This will install all templates from the `content` directory.

### Using the Templates

3. List available templates to verify installation:
   ```bash
   dotnet new list
   ```

4. Create a new project from your template:
   ```bash
   dotnet new <template-short-name> -o MyTestProject
   ```

5. Navigate to the created project and test it:
   ```bash
   cd MyTestProject
   dotnet build
   dotnet run
   ```

### Uninstalling Local Templates

When you're done testing or need to reinstall with changes:

```bash
dotnet new uninstall .
```

### Iterating on Templates

When testing template changes:

1. Make changes in the source template repository
2. Update the submodule to pull the latest changes:

   **For the default branch:**
   ```bash
   git submodule update --remote content/<TemplateName>
   ```

   **For a specific branch:**
   ```bash
   cd content/<TemplateName>
   git checkout <branch-name>
   git pull origin <branch-name>
   cd ../..
   ```

3. Uninstall the current local version:
   ```bash
   dotnet new uninstall .
   ```
4. Reinstall with the updated template:
   ```bash
   dotnet new install .
   ```
5. Test the updated template
6. If changes are good, commit the submodule update:
   ```bash
   git add content/<TemplateName>
   git commit -m "Update <TemplateName> to latest version"
   ```

## Publishing to NuGet

The repository uses GitHub Actions to automatically publish packages to NuGet.org.

### Setting Up NuGet API Key

1. Generate a NuGet API key:

2. Add the API key to GitHub repository secrets:
   - Navigate to your repository on GitHub
   - Go to **Settings** → **Secrets and variables** → **Actions**
   - Click **New Repository Secret**
   - Name: `NUGET_API_KEY`
   - Value: Paste your NuGet API key
   - Click **Add secret**

### Publishing Workflow

The GitHub Actions workflow (`.github/workflows/publish-alpha.yaml`) automatically triggers on:
- Pushes to the `alpha` branch
- Version tags matching `v*.*.*`

### Releasing a New Version

To publish a new version to NuGet:

1. Update template(s) via git submodule:
   ```bash
   cd content/<TemplateName>
   git pull origin main
   cd ../..
   ```

2. Bump the version in `SPC42.DotNetProjectTemplates.csproj`:
   ```xml
   <PackageVersion>N.N.N</PackageVersion>
   ```

3. Commit the submodule update and version bump:
   ```bash
   git add content/ SPC42.DotNetProjectTemplates.csproj
   git commit -m "Update templates and bump version to N.N.N"
   ```

4. Create a version tag matching the package version:
   ```bash
   git tag v0.2.4
   ```

5. Push the commit and tag to GitHub:
   ```bash
   git push origin main
   git push origin vN.N.N
   ```

The GitHub Actions workflow will automatically trigger and publish the package to NuGet.org.

**Note:** Ensure the tag version (`vN.N.N`) matches the `PackageVersion` in the `.csproj` file.