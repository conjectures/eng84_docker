
# The nginx official image will be used as base
FROM nginx

# optional
LABEL MAINTAINER = guy@example.com


COPY app /usr/share/nginx/html

# Expose port
EXPOSE 80

# Execute command
CMD ["nginx", "-g", "daemon off;"]
