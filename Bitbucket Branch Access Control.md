# Bitbucket Branch Access Control Guide

This guide explains how to **restrict developer access** to only the **beta branches** in a **Bitbucket repository** using **branch permissions**. By following this guide, you can ensure that only authorized users or teams have access to sensitive branches in your repository, such as `beta` branches.

## Prerequisites

- **Bitbucket repository** where you want to set up branch permissions.
- **Admin access** to the Bitbucket repository.

## Steps to Configure Branch Permissions

### Step 1: Log in to Bitbucket and Access the Repository

1. Go to [Bitbucket](https://bitbucket.org) and log in with your credentials.
2. Navigate to the **repository** where you want to restrict access to beta branches.
3. Click on the **gear icon** (Repository Settings) on the left sidebar.

### Step 2: Set Up Branch Permissions

1. In the **Repository Settings** page, scroll down to **Branch permissions** under **Workflow**.
2. Click **Branch permissions** to see the current branch permissions.

### Step 3: Add a Branch Permission for Beta Branches

1. Click on **Add permission**.
2. In the **Branch** field:
   - Type `beta` for the exact branch or `beta/*` to apply it to all `beta`-prefixed branches.
3. **Set Permissions**:
   - **Read**: Choose **Allow** to grant read access (optional).
   - **Write**: Select **Restrict** and add users or groups who are allowed to write to the branch.
   - **Merge**: Restrict merge access as necessary.
4. Click **Save**.

### Step 4: Restrict Access to Other Branches

1. Add specific permissions for branches like `main`, `master`, or any other sensitive branches.
2. Use wildcards (`*`) to restrict access to all branches except the allowed ones.

### Step 5: Verify and Test Permissions

1. After saving permissions, go back to the **Branch permissions** page and verify the restrictions.
2. **Test** the permissions by logging in as a developer who is not authorized to access `beta` branches and attempting to push changes.

## Conclusion

By following these steps, you've successfully configured **branch permissions** in **Bitbucket** to restrict developer access to only the **beta branches**. This will ensure that only authorized users or groups can work with sensitive branches.

## Troubleshooting

- **No permission to push to a branch**: Ensure the **user or group** has been correctly added under the **Write** permission for the specific branch.
- **Unable to merge changes**: Check the **Merge restrictions** for the branch in **Branch permissions**.

