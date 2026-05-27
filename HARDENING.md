# Hardening Report: helm--chart-releaser-action/v1.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **helm--chart-releaser-action/v1.7.0** was hardened automatically. 25 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in action.yml directly interpolates multiple `${{ inputs.* }}` expressions into shell command strings without first assigning them to environment variables. This allows an attacker who controls input values to inject arbitrary shell commands. Affected expressions include: `${{ inputs.charts_dir }}`, `${{ inputs.version }}`, `${{ inputs.config }}`, `${{ inputs.install_dir }}`, `${{ inputs.install_only }}`, `${{ inputs.skip_packaging }}`, `${{ inputs.skip_existing }}`, `${{ inputs.skip_upload }}`, `${{ inputs.mark_as_latest }}`, `${{ inputs.packages_with_index }}`, and `${{ inputs.pages_branch }}`. All should be passed via `env:` variables and referenced as `$VAR` in the shell.

Locations:

- `action.yml:67`

### github-env-injection (severity: high)

The `run:` block writes the attacker-controlled value `${{ inputs.install_dir }}` directly to `$GITHUB_PATH` via `echo ${{ inputs.install_dir }} >> "$GITHUB_PATH"` without any sanitization (missing `printf '%s' ... | tr -d '\n\r'` step). A malicious input containing newline characters can inject arbitrary entries into the runner's PATH, enabling path-hijacking attacks on subsequent steps.

Locations:

- `action.yml:87`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.charts_dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:76`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.version }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:78`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.version }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:79`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.config }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:82`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.config }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:83`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install_dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:86`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.version }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:87`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install_dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:91`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install_dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:92`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install_only }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:95`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install_only }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:96`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.skip_packaging }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:99`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.skip_packaging }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:100`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.skip_existing }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:103`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.skip_existing }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:104`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.skip_upload }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:107`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.skip_upload }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:108`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mark_as_latest }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:111`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mark_as_latest }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:112`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.packages_with_index }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:115`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.packages_with_index }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:116`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.pages_branch }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:119`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.pages_branch }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:120`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, static-inline-injection

**Notes:**

Fixed all findings in action.yml by: (1) Moving all ${{ inputs.* }} expressions (charts_dir, version, config, install_dir, install_only, skip_packaging, skip_existing, skip_upload, mark_as_latest, packages_with_index, pages_branch) from the run: shell block into the step's env: block as INPUT_* variables, then referencing them as plain shell variables in the script. (2) Fixed the github-env-injection by sanitizing both the user-provided INPUT_INSTALL_DIR and the auto-computed install path using 'printf "%s" "$VAR" | tr -d "\n\r"' before writing to $GITHUB_PATH.

