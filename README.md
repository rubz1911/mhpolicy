# DOCX Library — GitHub Pages (No Actions)

A static site that lists `.docx` files from the `library/` folder by calling the GitHub API at runtime. No GitHub Actions, no server.

## Deploy
1. Create a new GitHub repo.
2. Upload all files from this ZIP.
3. Edit `index.html` and set `OWNER`, `REPO`, and `BRANCH` in the **Config** block.
4. Enable **GitHub Pages**: Settings → Pages → Build from branch → `main` / root.
5. Upload your `.docx` into `library/` using GitHub’s “Upload files”.

## How it works
- The page calls:
  - `GET https://api.github.com/repos/<OWNER>/<REPO>/contents/library?ref=<BRANCH>` to list files.
  - `GET https://api.github.com/repos/<OWNER>/<REPO>/commits?path=library/<FILE>&per_page=1&sha=<BRANCH>` to get the last modified date.
- It renders a table and links downloads to the same site path: `./library/<file>.docx` (served by Pages).
- Results are cached in `localStorage` to tolerate API hiccups and rate limits.

## Notes
- Unauthenticated GitHub API limit is ~60 requests/hour per IP. Each file costs ~2 calls (list + 1 commit lookup). This is usually fine for small libraries.
- If your org blocks `api.github.com`, this approach won’t work; use the GitHub Actions–based version instead.
- Private repos won’t work without an API token. For private content, use Actions to pre-build a static `index.json`.
