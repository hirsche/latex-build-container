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

For your convenience, you'll find templates in this repository for *.gitigore*
as well as *.gitattributes* which are useful for LaTeX projects when pushed to
git repositories.