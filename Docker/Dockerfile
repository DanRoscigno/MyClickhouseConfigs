## Base ########################################################################
# Use a larger node image to do the build for native deps (e.g., gcc, python)
FROM node:lts as base

# Reduce npm log spam and colour during install within Docker
ENV NPM_CONFIG_LOGLEVEL=warn
# should not be needed, if DNS is out this will get npm working:
#ENV NPM_CONFIG_REGISTRY="http://104.16.23.35/"
ENV NPM_CONFIG_COLOR=false

# We'll run the app as the `node` user, so put it in their home directory
WORKDIR /home/node/app
# Do the chown so that the node_modules/.cache can be updated
RUN chown -R node:node /home/node

# Copy the source code over
COPY --chown=node:node . /home/node/app/
#COPY --chown=node:node babel.config.js /home/node/app/
#COPY --chown=node:node Dockerfile /home/node/app/
#COPY --chown=node:node docker-run /home/node/app/
#COPY --chown=node:node docs /home/node/app/
#COPY --chown=node:node docusaurus.config.js /home/node/app/
#COPY --chown=node:node LICENSE /home/node/app/
#COPY --chown=node:node package.json /home/node/app/
#COPY --chown=node:node README.md /home/node/app/
#COPY --chown=node:node sidebars.js /home/node/app/
#COPY --chown=node:node src /home/node/app/
#COPY --chown=node:node static /home/node/app/

## Development #################################################################
# Define a development target that installs devDeps and runs in dev mode
FROM base as development
WORKDIR /home/node/app
# Install (not ci) with dependencies, and for Linux vs. Linux Musl (which we use for -alpine)
RUN npm install
# Switch to the node user vs. root
USER node
# Expose port 3000
EXPOSE 3000
# Start the app in debug mode so we can attach the debugger
CMD ["npm", "start"]

## Production ##################################################################
# Also define a production target which doesn't use devDeps
FROM base as production
WORKDIR /home/node/app
COPY --chown=node:node --from=development /home/node/app/node_modules /home/node/app/node_modules
# Build the Docusaurus app
RUN npm run build

## Deploy ######################################################################
# Use a stable nginx image
FROM nginx:stable-alpine as deploy
WORKDIR /home/node/app
# Copy what we've installed/built from production
COPY --chown=node:node --from=production /home/node/app/build /usr/share/nginx/html/

