# Use ultra-lightweight nginx image
FROM nginx:mainline-alpine-slim

# Add maintainer and labels for traceability
LABEL environment="production"
LABEL tier="frontend"

# Performance optimization: Clean default files
RUN rm -rf /usr/share/nginx/html/*

# Copy our advanced UI
COPY index.html /usr/share/nginx/html/index.html

# Security: Expose standard port
EXPOSE 80

# Healthcheck to tell Kubernetes if the container is actually alive
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]