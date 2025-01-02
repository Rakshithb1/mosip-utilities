FROM alpine:3.18

ARG SOURCE
ARG COMMIT_HASH
ARG COMMIT_ID
ARG BUILD_TIME
LABEL source=${SOURCE}
LABEL commit_hash=${COMMIT_HASH}
LABEL commit_id=${COMMIT_ID}
LABEL build_time=${BUILD_TIME}

# Environment variables for container user configuration
ARG container_user=mosip
ARG container_user_group=mosip
ARG container_user_uid=1001
ARG container_user_gid=1001

# Install dependencies in a single RUN command to reduce layers
RUN apk update && apk add --no-cache bash postgresql-client-15 curl aws-cli

# Set working directory for the user
WORKDIR /home/${container_user}
ENV work_dir=/home/${container_user}

# Copy the backup script and ensure it's executable
COPY backup_script.sh /home/${container_user}/
RUN chmod +x /home/${container_user}/backup_script.sh

# Set environment variables
ENV DB_USER=postgres
ENV DB_HOST=
ENV DB_PORT=5432
ENV DB_PASSWORD=
ENV S3_BUCKET=s3-bucket-name
ENV S3_FOLDER=postgresbackup
ENV NUM_BACKUPS_TO_KEEP=5
ENV AWS_ACCESS_KEY_ID=
ENV AWS_SECRET_ACCESS_KEY=
ENV AWS_DEFAULT_REGION=

# Change ownership of the working directory
RUN chown -R ${container_user}:${container_user} /home/${container_user}

# Select the container user for all tasks
USER ${container_user_uid}:${container_user_gid}

# Default command to run the backup script
CMD ["bash", "./backup_script.sh"]
