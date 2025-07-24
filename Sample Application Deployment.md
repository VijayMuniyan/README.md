# Static HTML Deployment via GitHub Pages

## Step 1: Create a GitHub Repository

1. Go to [https://github.com](https://github.com) and log in.
2. Click the **+** icon (top-right) and select **New repository**.
3. Enter a repository name, e.g., `sample-static-site`.
4. Choose **Public**.
5. (Optional) Add a README.md or `.gitignore`.
6. Click **Create repository**.

---

Step 2: Create an index.html file with this sample content:

<!DOCTYPE html>
<html>
<head>
  <title>My Sample Static Site</title>
</head>
<body>
  <h1>Hello from GitHub Pages!</h1>
  <p>This is a static HTML page deployed via GitHub Pages.</p>
</body>
</html>

Step 3: Enable GitHub Pages
1.Go to your repository on GitHub.
2.Click on Settings.
3.In the left sidebar, click Pages.
4.Under Source, select branch: main and folder: / (root).
5.Click Save.
6.GitHub will build and publish your site. The URL will be shown on this page, typically:

https://<USERNAME>.github.io/<REPO>/


