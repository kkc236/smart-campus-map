# Admin Online Publishing

This project now supports real shared Admin updates on GitHub Pages.

## How It Works

The student map reads public campus data from:

```text
campus-data.json
```

The Admin console keeps a local draft while editing, then uses **Publish online** to write the current public event and place data to GitHub:

```text
public/campus-data.json
```

For repositories that still serve GitHub Pages from a `gh-pages` branch, the same JSON is also written to:

```text
campus-data.json
```

After publishing, students only need to refresh the online page to see the updated public map data.

## Admin Steps

1. Open the Admin console and log in.
2. Edit public events or public places.
3. Paste a GitHub fine-grained token into **GitHub token**.
4. Choose **Both live sites**, `kkc236 / smart-campus-map`, or `ENT208-GROUP-9 / smp_v2`.
5. Click **Publish online**.
6. Wait for the success message, then refresh the student page.

## Token Permission

Use a GitHub fine-grained token with:

```text
Repository contents: Read and write
```

The token is stored only in the current browser tab session. It is not committed to the codebase.

## Important Boundary

Admin publishing updates shared public campus events and places.

Personal Space data remains private to the current student browser and is not published to GitHub.
