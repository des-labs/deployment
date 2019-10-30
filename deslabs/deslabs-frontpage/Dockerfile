#mgckind/frontpage:2.0.0
FROM nginx:1.14-alpine
RUN apk add --no-cache git openssh
ADD https://api.github.com/repos/des-labs/frontpage/git/refs/heads/dev version.json
RUN git clone --single-branch --branch dev https://github.com/des-labs/frontpage.git
RUN cp -r frontpage/public_html/* /usr/share/nginx/html
RUN cp frontpage/deslabs.conf /etc/nginx/conf.d/
#RUN rm /etc/nginx/conf.d/default.conf
