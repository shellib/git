# Git Library for Shellib

A bash library providing git repository management utilities for URL parsing, cloning, and workspace organization.

## Usage

```bash
# Load the library using grab
if [ ! -f "$HOME/bin/grab" ]; then
  mkdir -p $HOME/bin
  export PATH=$PATH:$HOME/bin
  curl -o $HOME/bin/grab -L https://github.com/shellib/grab/raw/master/grab.sh && chmod +x $HOME/bin/grab
fi

source $(grab github.com/shellib/git as git)
```

## Functions

### URL Parsing
- `git::extract_host(url)` - Extract hostname from git repository URL
- `git::extract_organization(url)` - Extract organization/owner name
- `git::extract_repository(url)` - Extract repository name
- `git::extract_path(url)` - Extract path after repository
- `git::extract_github_pr_number(url)` - Extract GitHub PR number
- `git::extract_gitlab_mr_number(url)` - Extract GitLab MR number

### URL Building
- `git::build_ssh_url(host, org, repo, [username])` - Build SSH clone URL
- `git::build_https_url(host, org, repo)` - Build HTTPS clone URL
- `git::workspace_path(host, org)` - Build workspace directory path

### Cloning
- `git::clone(host, org, repo, target_dir)` - Smart clone using config
- `git::clone_with_fallback(host, org, repo, target_dir, [username])` - Clone with SSH->HTTPS fallback

### Configuration
- `git::get_method(host, org, repo)` - Get configured clone method
- `git::get_username(host, org, repo)` - Get configured username
- `git::get_config_values(host, org, repo)` - Get method and username

### Utilities
- `git::is_github(url)` - Check if URL is from GitHub

## Configuration

The library supports TOML configuration files (default: `~/.config/qutebrowser/git.toml`):

```toml
[default]
method = "fallback"

["github.com"]
method = "ssh"

["gitlab.example.com/org/special-repo"]
method = "https"
username = "myuser"
```

Configuration hierarchy (most specific wins):
1. Repository-specific: `["host/org/repo"]`
2. Host-specific: `["host"]` 
3. Default: `[default]`

## Testing

```bash
./library.sh --test
```

## Dependencies

- [shellib/toml](https://github.com/shellib/toml) - TOML parsing library