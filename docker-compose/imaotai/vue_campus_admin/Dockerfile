FROM nginx:stable-alpine

# 复制html文件到路径
COPY dist /usr/share/nginx/html

# 复制conf文件到路径
COPY ./nginx/conf/nginx.conf /etc/nginx/nginx.conf
