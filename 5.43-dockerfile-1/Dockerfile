# use this empty Dockerfile to build your assignment
# NOTE: The instructions are now in the README.md file in this directory.

# use node 6
FROM node:6-alpine

# open port 80 and 300
EXPOSE 80 3000

# install tini
RUN apk add --no-cache tini

# change workdir
WORKDIR /usr/src/app

# copy
COPY package.json package.json

# install
RUN npm install && npm cache clean --force

# copy all
COPY . .

# run
CMD ["node", "./bin/www"]