# Updating Academy Theme

This guide provides comprehensive instructions for updating the Academy Theme when changes are introduced in the main repository.

## Important Note

This guide is designed for **content repositories** (like `academy-example`) that use the Layer5 Academy Theme as a dependency. 

- **Content Repository**: This contains your learning content and courses
- **Theme Repository**: [`layer5io/academy-theme`](https://github.com/layer5io/academy-theme) contains all visual elements, layouts, CSS, and Hugo logic

For detailed information about creating learning paths, see the [official documentation](https://docs.layer5.io/cloud/academy/creating-your-learning-path/).

## Table of Contents

- [Prerequisites](#prerequisites)
- [Update Methods](#update-methods)
- [Step-by-Step Update Process](#step-by-step-update-process)
- [Version Management](#version-management)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Rollback Procedures](#rollback-procedures)

## Prerequisites

Before updating the theme, ensure you have:

- **Hugo** installed (check with `hugo version`)
- **Go** installed (check with `go version`)
- **Git** access to your site repository
- **Docker** (optional, for containerized development)
- **Backup** of your current working site (recommended)

## Update Methods

### Method 1: Update to Latest Version

This method updates to the most recent version of the theme:

```bash
# Navigate to your Hugo site directory
cd your-academy-site/

# Update to latest version
go get -u github.com/layer5io/academy-theme

# Clean module cache
hugo mod clean

# Clean and rebuild site
make clean
```

### Method 2: Update to Specific Version

If you need to update to a specific version:

```bash
# Update to specific version (replace v1.2.3 with desired version)
go get github.com/layer5io/academy-theme@v1.2.3

# Clean and rebuild
hugo mod clean
make site
```

## Step-by-Step Update Process

### 1. Pre-Update Preparation

```bash
# Create a backup of your current site
cp -r your-academy-site/ your-academy-site-backup/

# Check current theme version
grep "github.com/layer5io/academy-theme" go.mod

# Check for any local customizations
git status
```

### 2. Check for Breaking Changes

Before updating, review:
- [Release notes](https://github.com/layer5io/academy-theme/releases)
- Check repository for CHANGELOG.md or release notes
- Look for any migration guides in the repository documentation

### 3. Perform the Update

```bash
# Update the theme
go get -u github.com/layer5io/academy-theme

# Verify the update
go list -m github.com/layer5io/academy-theme
```

### 4. Clean and Rebuild

```bash
# Clean Hugo's module cache
hugo mod clean

# Clean build artifacts manually (if needed)
rm -rf public/ resources/

# Rebuild the site
make site
```

### 5. Test the Updated Site

```bash
# Start development server
make site

# Test your changes
make run-tests

# Alternative: Use Docker for testing
make docker

# Test key functionality:
# - Homepage loads correctly
# - Navigation works
# - Content renders properly
# - Search functionality (if applicable)
# - Mobile responsiveness
```

### 6. Commit Changes

```bash
# Add updated go.mod and go.sum
git add go.mod go.sum

# Commit the update
git commit -m "Update academy-theme to latest version"

# Push changes
git push origin main
```

## Version Management

### Checking Current Version

```bash
# Check current version in go.mod
cat go.mod | grep academy-theme

# Check all module versions
go list -m all | grep academy-theme
```

### Version Pinning

To prevent automatic updates to unwanted versions:

```bash
# Pin to specific version in go.mod
go mod edit -require=github.com/layer5io/academy-theme@v1.2.3
```

### Viewing Available Versions

```bash
# List all available versions
go list -m -versions github.com/layer5io/academy-theme
```

## Troubleshooting

### Common Issues and Solutions

#### Issue: "Module not found"
```bash
# Solution: Ensure module path is correct
go mod download github.com/layer5io/academy-theme
```

#### Issue: "Checksum mismatch"
```bash
# Solution: Clean and re-download
go clean -modcache
go get github.com/layer5io/academy-theme
```

#### Issue: "Hugo build fails after update"
```bash
# Solution: Check Hugo version compatibility
hugo version
# Update Hugo if needed
# Clear cache and rebuild
rm -rf resources/ public/
hugo mod clean
make site
```

#### Issue: "Styles not loading correctly"
```bash
# Solution: Clear browser cache and Hugo cache
hugo mod clean
rm -rf resources/ public/
make site
# Hard refresh browser (Ctrl+F5 or Cmd+Shift+R)
```

#### Issue: "Local development setup broken"
```bash
# Solution: Verify replace directive in go.mod
# Ensure local theme path is correct
replace github.com/layer5io/academy-theme => ../academy-theme
```

### Getting Help

If you encounter issues:

1. **Check the logs**: Run `make site` for detailed output
2. **Run tests**: Use `make run-tests` to verify functionality
3. **Search existing issues**: Check [GitHub Issues](https://github.com/layer5io/academy-theme/issues)
4. **Create an issue**: Provide Hugo version, theme version, and error details
5. **Join community**: Reach out on [Layer5 Slack](http://slack.layer5.io)

## Best Practices

### Regular Updates

- **Weekly check**: Monitor for new releases
- **Staging environment**: Test updates before applying to production
- **Automated notifications**: Set up GitHub watch for releases

### Safe Update Workflow

1. **Always backup** before updating
2. **Read release notes** before updating
3. **Test locally** before deploying
4. **Update dependencies** one at a time
5. **Document changes** in your project

### Monitoring Theme Changes

```bash
# Watch for theme repository changes
# Add to your CI/CD or use GitHub notifications

# Check for updates programmatically
go list -u -m github.com/layer5io/academy-theme
```

## Rollback Procedures

### Quick Rollback to Previous Version

```bash
# Check git history for previous version
git log --oneline | grep academy-theme

# Rollback to specific commit
git checkout <commit-hash> -- go.mod go.sum

# Or rollback to specific version
go get github.com/layer5io/academy-theme@v1.1.0

# Clean and rebuild
hugo mod clean
make site
```

### Emergency Rollback

If the site is completely broken:

```bash
# Restore from backup
rm -rf your-academy-site/
cp -r your-academy-site-backup/ your-academy-site/

# Or use git to reset
git reset --hard HEAD~1  # Revert last commit
```

## Automation

### GitHub Actions Example

Create `.github/workflows/update-theme.yml`:

```yaml
name: Check Theme Updates
on:
  schedule:
    - cron: '0 9 * * 1'  # Weekly on Monday
  workflow_dispatch:

jobs:
  check-updates:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-go@v3
        with:
          go-version: '1.19'
      - name: Check for updates
        run: |
          go list -u -m github.com/layer5io/academy-theme
          # Add notification logic here
```

---

## Need More Help?

- **Documentation**: [Main README](README.md)
- **Contributing**: [CONTRIBUTING.md](CONTRIBUTING.md)  
- **Issues**: [GitHub Issues](https://github.com/layer5io/academy-theme/issues)
- **Discussions**: [GitHub Discussions](https://github.com/layer5io/academy-theme/discussions)