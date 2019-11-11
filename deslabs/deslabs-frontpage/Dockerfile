#mgckind/frontpage:2.0.0
FROM nginx:1.14-alpine
RUN apk add --no-cache git openssh
# Download version info to ensure Docker image cache does not skip the source
# file repo git clone operation when the des-labs/frontpage repo has been updated.
#ADD https://api.github.com/repos/des-labs/frontpage/git/refs/heads/dev version.json
#RUN git clone --single-branch --branch dev https://github.com/des-labs/frontpage.git
# Copy in web root and nginx config files from git submodule
COPY src-repo/public_html/ /usr/share/nginx/html/
COPY src-repo/deslabs.conf /etc/nginx/conf.d/
