---
title: docker-system-prune-command-reference
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---
## Description

The `docker system prune` command is a powerful tool for cleaning up unused Docker resources, helping you reclaim disk space and maintain a tidy Docker environment. Here's a detailed explanation of the command and the specific role of the `-a` and `-f` flags:

## Basic Command: `docker system prune`

- **Purpose:** Removes unused Docker objects, including:
  - **Stopped containers**
  - **Unused networks**
  - **Dangling images** (images not tagged and not referenced by any container)
  - **Build cache**

- **Default Behavior:** Before performing the cleanup, Docker prompts for confirmation to ensure you don't accidentally remove important resources.

## Adding the `-a` Flag: `docker system prune -a`

- **Flag:** `-a` or `--all`
- **Purpose:** Extends the cleanup to include **all unused images**, not just dangling ones. This means any image not currently referenced by a container will be removed, regardless of whether it's tagged.

- **Use Case:** Useful when you want a more comprehensive cleanup, especially if you've built or pulled many images that are no longer needed.

**Example:**
```bash
docker system prune -a
```

## Adding the `-f` Flag: `docker system prune -a -f`

- **Flag:** `-f` or `--force`
- **Purpose:** **Bypasses the confirmation prompt**, allowing the command to run non-interactively. This is particularly useful in scripts or automated environments where manual confirmation isn't feasible.

- **How to Add:** Simply include the `-f` flag in your command alongside other flags.

**Example:**
```bash
docker system prune -a -f
```

**What Happens with `-f`:**
- **No Confirmation Needed:** Docker will immediately proceed to remove the specified unused resources without asking for user confirmation.
- **Caution Advised:** Since this bypasses the safety prompt, ensure you're certain about the cleanup to avoid accidentally removing needed resources.

## Summary of Flags

| Flag | Short Form | Description                                          |
|------|------------|------------------------------------------------------|
| `-a` | `--all`    | Remove **all** unused images, not just dangling ones.|
| `-f` | `--force`  | **Force** the prune operation without confirmation.  |

## Complete Command Usage

To perform a comprehensive cleanup without any confirmation prompts, you can combine both flags as follows:

```bash
docker system prune -a -f
```

**Breakdown:**
- `docker system prune`: Initiates the cleanup process.
- `-a`: Includes all unused images in the cleanup.
- `-f`: Forces the operation without asking for confirmation.

## Practical Considerations

- **Disk Space Reclamation:** Regularly pruning unused Docker objects can help manage disk space, especially in development environments where images and containers can accumulate quickly.
  
- **Automation:** When integrating Docker cleanup into CI/CD pipelines or scheduled maintenance scripts, using the `-f` flag ensures the process runs smoothly without manual intervention.

- **Safety:** Always ensure that the images and containers you're removing are genuinely unused to prevent accidental loss of important data or environments.

## Additional Resources

- **Docker Documentation:** For more detailed information, refer to the [official Docker documentation on system pruning](https://docs.docker.com/engine/reference/commandline/system_prune/).
  
- **Command Help:** Use the `--help` flag to explore more options and get immediate assistance with the command.

```bash
docker system prune --help
```

This command will display all available options and their descriptions, providing further insight into Docker's pruning capabilities.

---

**Caution:** Always double-check which resources will be removed, especially when using the `-a` and `-f` flags, to avoid unintended data loss.