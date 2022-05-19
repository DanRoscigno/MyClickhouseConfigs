### Dockerfile

The Dockerfile comes from https://github.com/amadeu01 in this
[GitHub issue](https://github.com/googlecodelabs/tools/issues/635).

### Instructions:

- create an empty working dir
- copy the above Dockerfile to the working dir
- create a docs dir with a subdir of your choosing.  I used mkdir -p docs/foo
- add one or more markdown files to docs/foo/
- run docker build -t docs:codelab . to build an image tagged as docs:codelab.  This image will copy in the docs folder and build HTML from the markdown files that you provided.  It also grabs the googlecodelabs/tools repo and uses the landing page from that repo
- run docker run -p 80:80/tcp docs:codelab to run the image and expose the content at http://localhost/
- browse to localhost

Note: If you do not have any markdown files in a subdir of the docs dir you will be prompted to authenticate and give access to your Google Docs.  I could not make this work, but I prefer markdown so it was not an issue for me.
