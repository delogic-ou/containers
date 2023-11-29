FROM alpine:3 as apps

ARG TARGETARCH=${TARGETARCH:-amd64}
ARG TERRAFORM_VERSION=${TERRAFORM_VERSION:-1.2.1}
ARG TERRAGRUNT_VERSION=${TERRAGRUNT_VERSION:-0.43.1}

RUN wget -q https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_${TARGETARCH}.zip && \
  unzip terraform_${TERRAFORM_VERSION}_linux_${TARGETARCH}.zip && \
  wget -q https://github.com/gruntwork-io/terragrunt/releases/download/v${TERRAGRUNT_VERSION}/terragrunt_linux_${TARGETARCH} -O terragrunt


FROM gcr.io/google.com/cloudsdktool/google-cloud-cli:alpine

LABEL org.opencontainers.image.authors="sre@delogic.io" \
      org.opencontainers.image.description="Docker image for CI/CD GCloud builds" \
      org.opencontainers.image.licenses="Apache-2.0" \
      org.opencontainers.image.ref.name="alpine:3" \
      org.opencontainers.image.source="https://github.com/delogic-io/containers" \
      org.opencontainers.image.title="gcloud-cli-tf" \
      org.opencontainers.image.vendor="Delogic.io"

ENV DEBIAN_FRONTEND=noninteractive

COPY --from=apps /terraform ./usr/bin/terraform
COPY --from=apps /terragrunt ./usr/bin/terragrunt

SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN APPS=( \
      "terraform" \
      "terragrunt" \
    ) && \
    for i in "${APPS[@]}"; do \
      chmod +x /usr/bin/${i}; done

CMD ["/bin/bash"]
