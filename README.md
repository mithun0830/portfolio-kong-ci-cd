# Kong API Gateway Deployment Workflow

This repository contains a GitHub Actions workflow for deploying API specifications to Kong API Gateway using deck.

## Directory Structure

```
.
├── .github/
│   └── workflows/
│       └── kong-deploy.yml    # GitHub Actions workflow
├── kong/
│   ├── declarative/          # Generated Kong declarative configs
│   └── specs/                # OpenAPI/Swagger specifications
│       └── profile_swagger_v1.0.yaml
└── README.md
```

## Workflow Features

- **Automatic Validation**: Validates swagger files before deployment
- **Declarative Configuration**: Converts swagger to Kong's declarative format
- **Manual Approval**: Requires approval before deploying to Kong
- **Tagged Deployment**: Uses 'profilebuilder' tag for selective deployment
- **Safety Checks**: 
  - Kong health check
  - Configuration diff preview
  - Error handling
  - Artifact management

## Usage

1. **Make Changes**:
   - Update swagger files in `kong/specs/` directory
   - Commit and push to the main branch

2. **Workflow Execution**:
   - Workflow triggers automatically on push to main
   - Validates and converts swagger to declarative format
   - Shows diff of changes to be applied

3. **Deployment**:
   - Review changes in GitHub Actions interface
   - Approve deployment when ready
   - Monitor deployment status

4. **Verification**:
   - Check Kong Admin API for applied changes
   - Verify API endpoints are working as expected

## Kong Configuration

- **Admin URL**: http://34.55.116.90:8001
- **Tag**: profilebuilder

## Error Handling

The workflow includes comprehensive error handling:
- Validates swagger file format
- Checks Kong connectivity
- Verifies declarative configuration
- Monitors deployment status

## Artifacts

The workflow uses GitHub Actions artifacts to:
- Store converted declarative configurations
- Pass configurations between jobs
- Maintain deployment history

## Manual Steps

If you need to run deck commands manually:

```bash
# Convert swagger to declarative
deck file openapi2kong --spec kong/specs/profile_swagger_v1.0.yaml --output-file kong/declarative/profilebuilder.yml

# Check diff
deck diff --state kong/declarative/profilebuilder.yml --kong-addr http://34.55.116.90:8001

# Deploy
deck sync --state kong/declarative/profilebuilder.yml --kong-addr http://34.55.116.90:8001 --select-tag profilebuilder
