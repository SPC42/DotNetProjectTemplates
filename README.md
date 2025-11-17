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

3. Commit the submodule addition:
   ```bash
   git add .gitmodules content/<TemplateName>
   git commit -m "Add <TemplateName> template"
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