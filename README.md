# latex-container

Dockerfile for creating a LaTeX image with a lot of tools necessary to build
LaTeX documents. It's based on *texlive-full* and supports e.g. minted, xfig
utils and more.

This image is used in GitLab CI to build LaTeX documents but also can be used
locally if you do not want to install LaTeX on your machine.

Kindly change the Dockerfile to your needs.

## Build'n Publish

To build the image, run the following command:

```bash
docker build -t hirsche/latex .
```

To publish the image, at a registry (currently on Docker Hub)

```bash
docker push hirsche/latex:latest
```

Replace `latest` with specific version tag if necessary. Of course, you need to
be logged in to the registry as well as having created a repository on the
respective registry. Note: Docker registries work similar to Git repositories
but contain docker images instead of code.

**Important note:** You do not need to build the image by yourself. The current
version is published on Docker Hub and can be used directly.
```bash
docker pull hirsche/latex
```

## Usage Example in GitLab CI

In order to build a LaTeX document in GitLab CI, need to add `.gitlab-ci.yml`
with the following content:

```yaml
build_pdf:
  tags:
    - wiener
  image: hirsche/latex
  script:
    - latexmk -pretex="\def\commitsha{${CI_COMMIT_SHORT_SHA}}" -usepretex ${file}
  artifacts:
    paths:
      - <NameOfCreatedPdfFile>.pdf
    expire_in: 2 week
```

Take care to replace `<NameOfCreatedPdfFile>` with the name of the file that
is created by the LaTeX document.

In the LaTeX document, you can use the `\commitsha` macro to print the commit
hash. To ensure that local builds work, you may what to wrap the macro in a
`\ifdefined` statement like this:

```latex
\ifdefined\commitsha
  \commitsha
\fi
```

## Other Notes

For **Linking** to the latest version of a PDF file in GitLab, you can use the
one of following links in Markdown

```markdown	
[Show PDF!](https://git.isia.fh-salzburg.ac.at/path/to/repo/-/jobs/artifacts/main/browse?job=build_pdf)

or a direct download link with an image:

[![img](https://img.shields.io/badge/download-pdf-green)](https://git.isia.fh-salzburg.ac.at/path/to/repo/-/jobs/artifacts/master/raw/nameoffile.pdf?job=build_pdf)
```

Replace `path/to/repo` with the path to your repository and `nameoffile` with
the name of the PDF-file.

For your convenience, you'll find **templates** in this repository for
*.gitigore* as well as *.gitattributes* which are useful for LaTeX projects when
pushed to git repositories.

## TODO:

- pull the latest version of
  [bibtex](https://git.isia.fh-salzburg.ac.at/publications/bibtex) in order to
  be able to use commonly shared references