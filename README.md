# Portfolio Template

A starter repo for building and validating a personal portfolio webpage.

![Pipeline Status](your-badge-url-here)

## Structure

```
.
├── index.html                     # Your portfolio page (edit this)
├── style.css                      # Stylesheet (customize this)
├── scripts/
│   └── validate.py                # Checks your portfolio meets requirements
├── .htmlhintrc                    # HTMLHint linting rules
├── .github/
│   └── workflows/
│       └── pipeline.yml          # CI pipeline (you create this in Part 2)
└── README.md
```

## Running the validator locally

```bash
python3 scripts/validate.py
```

The script checks `index.html` against the requirements and prints `[PASS]`
or `[FAIL]` for each.

## CI pipeline

Create `.github/workflows/pipeline.yml` yourself. The file is
intentionally empty. Your pipeline should run `htmlhint` and
`scripts/validate.py` on every push.

## Short Answers:

- When I run git push, the update to the main branch is detected and triggers the workflow. The first job, lint, runs to ensure the index.html file has no syntax or formatting issues. If linting succeeds, then the validate job runs and uses the validate.py file to ensure all portfolio sections are present and have the necessary information. Once validation passes, the deploy job runs, uploading the files as an artifact and publishing them using GitHub Pages. After deployment completes, GitHub Pages updates the live site with any changes made.
- The deploy job contains an if condition so that it only runs when push events happen on the main branch, preventing deployments from PRs or pushes to other branches. This is important because we would only want the site to be updated when we push a change to the main/production branch of the repository. Even though linting and validation would still run on other pushes or PRs, deployment must be restricted to solely run on pushes to the main branch. Without this check, we could end up breaking our production version of the site with a PR that has incomplete work.
- When I removed the #projects section, the validate job failed at the "Validate Portfolio Structure" step because validate.py detected that there was a missing section. Since the validate job failed, the deploy job was automatically skipped due to its dependency (needs: validate) on successful validation. The lint job passed because the index.html syntax was still valid, This failure prevented the broken portfolio from being deployed to GitHub Pages. This is a behavior that we would hope to see, because we wouldn't want the deployment to go through if we accidentally removed something. For example, it would be as if Amazon accidentally removed the "Add to Cart" button from their website and pushed, then everyone visiting the website wouldn't be able to buy anything.
