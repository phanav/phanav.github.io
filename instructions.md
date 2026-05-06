# Quarto Blog Publishing Workflow

Based on the repository configuration, this blog is built using **Quarto**. Here is the end-to-end workflow to add a new blog post via a Jupyter Notebook and have it automatically deployed:

### 1. Create the Post
Quarto looks for content inside the `posts/` directory (as configured in `index.qmd`).
*   **Create a new folder** for your post inside the `posts/` directory (e.g., `posts/my-new-post/`).
*   **Add your Jupyter Notebook** (`.ipynb` file) inside this new directory.
*   **Add YAML Frontmatter**: The first cell in your notebook should be a Markdown cell (or Raw cell) containing the YAML frontmatter to define the post's metadata. It should look something like this:
    ```yaml
    ---
    title: "Your Post Title"
    author: "Phan Anh"
    date: "2024-05-06"
    categories: [machine learning, optimization]
    ---
    ```
    *Note: Your `posts/_metadata.yml` automatically applies `freeze: true` and `title-block-banner: true` to all posts, so you don't need to specify those.*

### 2. Render locally to the `docs` folder (Optional)
Your `_quarto.yml` specifies `output-dir: docs`. 
*   You can preview your website locally as you edit the notebook by running:
    ```bash
    quarto preview
    ```
*   To manually build the static HTML files into the `docs/` folder, run:
    ```bash
    quarto render
    ```

### 3. Commit and Push to Trigger Automatic Deployment
You have a GitHub Action set up in `.github/workflows/publish.yml` that handles the automatic deployment.
*   Once you are happy with the notebook, simply **commit your changes** (the new folder and `.ipynb` file in `posts/`) to the `main` branch.
*   **Push to GitHub**:
    ```bash
    git add posts/my-new-post/
    git commit -m "Add new blog post about X"
    git push origin main
    ```
*   **What happens next?** Pushing to the `main` branch automatically triggers the `Quarto Publish` GitHub Action. The action will spin up an Ubuntu runner, install Quarto, render your site, and then push the final HTML output directly to the `gh-pages` branch on your repository. GitHub Pages will then automatically serve the updated website.
