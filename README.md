# The Deterministic Dependency Reset (DDR)

> Your lockfile is not the absolute truth; it is a history of mistakes.

## The Problem: The "History of Mistakes"

Standard routines like `npm install` and `npm ci` are fundamentally flawed for long-term project health. They rely on the `package-lock.json` as an absolute truth, but in a long-lived project, that lockfile is rarely truth, it is a history of mistakes.

`npm ci` is a Mirror. It faithfully replicates every drift, every ghost dependency, and every bloated resolution from your lockfile into your build. It doesn't fix a broken state; it scales it.

## The Protocol

Run this sequence to purge "lock drift" and re-establish a high-integrity baseline:

```bash
# 1. The Nuclear Reset: Clear out historical baggage
rm -rf node_modules package-lock.json;

# 2. Baseline Establishment: Initial resolution
npm install;

# 3. The Push: Bump to latest allowed semver ranges
npm update;

# 4. The Reconciliation: Nuke the "dirty" state created by update
rm -rf node_modules package-lock.json;

# 5. The Final Truth: One final, vacuum-sealed resolution
npm install;
```

or

```bash
rm -rf node_modules package-lock.json;npm install;npm update;rm -rf node_modules package-lock.json;npm install;
```

## Why this beats the "Standard"

1. The Vacuum Principle

- By nuking the lockfile, you force the package manager to re-calculate the entire dependency graph from a vacuum. This ensures your environment matches your package.json intent, not your historical install baggage.

2. The Logic of the Double Delete

- The genius lies in the second `rm -rf`.
- **The Update Phase:** `npm update` is an iterative process. It moves dependencies to their latest allowed versions, but it can leave the tree in a "dirty" state where metadata (like `dev: true` flags) is inconsistent.
  - Note: Ensure your `package.json` uses semver ranges (like ^ or ~) that you trust, as this protocol will pull the latest versions allowed by those ranges.
- **The Reconciliation Phase:** By nuking the state after the update, you force one final, clean-slate resolution. This produces the most efficient, condensed, and verified version of the tree possible.

## What is Lock Drift?

This protocol doesn't trust the existing state. It forces the package manager to re-calculate the entire graph from a vacuum. It is the only way to ensure that your environment matches your `package.json` requirements while purging years of "ghost" sub-dependencies.

## The Golden Rule: Analyze -> Reset -> Verify

The protocol is only half the battle. You must prove the health of the new state.

1. Analyze Before: Capture bundle size (`vite-bundle-visualizer`) and other metrics like CSS health (wallace-cli).
2. Reset: Run the DDR Protocol.
3. Analyze After: Verify the reduction in "Unique Colors," "Unique Declarations," and total Transferred Size.
