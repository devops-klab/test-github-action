###############################
# Builder / test stage
#
# Use an nginx base image to validate the configuration during the build.
# If you later add custom configuration files under, for example, ./conf.d,
# copy them into this stage before the `nginx -t` command so the syntax check
# runs as part of `docker build`.
###############################
FROM nginx:1.27-alpine AS test

# Validate the (possibly customised) nginx configuration at build time.
RUN nginx -t


###############################
# Runtime stage
###############################
FROM nginx:1.27-alpine

# Copy the validated configuration forward from the test stage. Adjust or add
# COPY instructions if you provide your own config or static assets.
COPY --from=test /etc/nginx/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

# Basic health check to ensure nginx stays up. BusyBox's wget is available in
# the Alpine-based image, so we leverage it for a lightweight probe.
HEALTHCHECK CMD wget -qO- http://127.0.0.1:80/ > /dev/null 2>&1 || exit 1

CMD ["nginx", "-g", "daemon off;"]

