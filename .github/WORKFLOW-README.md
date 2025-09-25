# GitHub Actions Workflows

## Deployment Strategy

This repository uses GitHub Actions to automatically deploy the DIIP documentation to GitHub Pages with **simultaneous deployments** maintained for different branches and pull
requests.

### Branch-based Deployment

| Branch | Deployment Location | URL Pattern |
|--------|-------------------|-------------|
| `main` | Root directory (`/`) | `https://FIDEScommunity.github.io/DIIP` |
| `develop` | `/draft` directory | `https://FIDEScommunity.github.io/DIIP/draft` |
| Pull Requests | `/pr/pr-{number}` directory | `https://FIDEScommunity.github.io/DIIP/pr/pr-123` |

### Simultaneous Deployment Approach

**Key Feature**: All deployments are maintained **simultaneously** - deploying to one branch does NOT overwrite others.

The workflow manually manages the `gh-pages` branch instead of using GitHub's standard `deploy-pages` action, which would overwrite the entire site. This ensures:

- Main branch deployment preserves draft and PR previews
- Develop branch deployment preserves main and PR previews
- PR deployments preserve all other deployments
- Each deployment only updates its specific directory

### Workflow Features

#### Main Branch (`main`)

- Deploys to the **root** of GitHub Pages (`/`)
- Also creates a copy at `/latest` for backwards compatibility
- **Preserves** all existing `/draft` and `/pr/` subdirectories
- Represents the stable, published version of DIIP

#### Develop Branch (`develop`)

- Deploys to `/draft` subdirectory **only**
- **Preserves** root content and all `/pr/` directories
- Creates a commit status on the latest commit with the preview URL
- Used for reviewing upcoming changes before they go to main

#### Pull Requests

- Each PR gets its **own isolated** subdirectory: `/pr/pr-{number}`
- **Preserves** all other deployments (main, draft, other PRs)
- Comments on the PR with the preview URL
- Updates the comment when new commits are pushed
- Automatically cleans up **only its own** directory when closed/merged

### Implementation Details

#### Manual gh-pages Management

```bash
# The workflow:
1. Checks out both source branch and gh-pages branch
2. Updates only the target directory (., draft/, or pr/pr-N/)
3. Preserves all other directories
4. Commits and pushes only the specific changes
```

#### Smart Directory Handling

- **Root deployment** (main): Updates `*.html`, `assets/`, etc. but preserves `draft/` and `pr/` subdirs
- **Subdirectory deployment**: Only updates the specific subdirectory
- **Cleanup**: Only removes the specific PR directory when closed

### File Structure After Multiple Deployments

```
gh-pages branch:
├── README.md               (setup instructions)
├── index.html              (main branch - stable)
├── latest.html             (copy of main branch)
├── assets/                 (main branch assets)
├── custom-assets/          (main branch assets)
├── draft/                  (develop branch)
│   ├── index.html
│   ├── assets/
│   └── custom-assets/
└── pr/                     (PR previews)
    ├── pr-123/             (PR #123)
    │   ├── index.html
    │   ├── assets/
    │   └── custom-assets/
    ├── pr-456/             (PR #456)
    │   ├── index.html  
    │   ├── assets/
    │   └── custom-assets/
    └── pr-789/             (PR #789)
        ├── index.html
        ├── assets/
        └── custom-assets/
```

### Permissions Required

The workflow requires the following permissions:
- `contents: write` - To manage the gh-pages branch manually
- `pull-requests: write` - To comment on pull requests

### Setup Requirements

#### **Manual GitHub Pages Setup Required**

Due to GitHub API permission limitations, GitHub Pages must be enabled manually:

1. **One-time setup**: After the first workflow run:
    1. Go to repository **Settings > Pages**
    2. Under **Source**, select **Deploy from a branch**
    3. Select branch **gh-pages** and folder **/ (root)**
    4. Click **Save**

2. **Verification**: The workflow will automatically detect if Pages is enabled and provide setup instructions if needed

3. **Branch Protection** (recommended): Protect the `main` branch to ensure only reviewed changes are deployed to production

#### Automatic Features

- **Creates gh-pages branch** if it doesn't exist
- **Provides setup instructions** in the branch README
- **Detects Pages status** and shows appropriate messages
- **Workflow summaries** with setup guidance when needed

### Concurrent Deployment Safety

- **Concurrency Group**: `pages-${{ github.ref }}-${{ github.event.pull_request.number }}`
- Each branch/PR has its own deployment queue
- No conflicts between simultaneous deployments to different paths
- Safe parallel deployments of different branches/PRs

### Troubleshooting

#### "Resource not accessible by integration (403)"

This error occurs when trying to enable GitHub Pages via API. **Solution**: Enable Pages manually as described above.

#### "404 - Page not found"

This usually means GitHub Pages isn't enabled yet. **Solution**: Follow the manual setup instructions above.

#### "Deployment successful but site not updating"

GitHub Pages may take a few minutes to update after deployment. Check the Pages settings to ensure it's pointing to the `gh-pages` branch.

### Maintenance

The workflow handles all maintenance automatically:

- **No overwrites**: Each deployment only touches its own directory
- **Automatic cleanup**: PR directories removed when PRs close
- **Preserves history**: All deployments remain available
- **Self-healing**: Creates gh-pages branch if missing
- **Idempotent**: Safe to re-run without side effects
- **Clear feedback**: Provides setup instructions when needed