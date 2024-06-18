ARG TARGETARCH=${TARGETARCH:-amd64}
ARG TFLINT_VERSION=${TFLINT_VERSION:-0.44.1}
ARG TERRAFORM_VERSION=${TERRAFORM_VERSION:-1.2.1}
ARG TERRAGRUNT_VERSION=${TERRAGRUNT_VERSION:-0.44.1}
ARG PRECOMMIT_VERSION=${PRECOMMIT_VERSION:-3.2.2}

FROM alpine:3 as apps

ARG TARGETARCH
ARG TFLINT_VERSION
ARG TERRAFORM_VERSION
ARG TERRAGRUNT_VERSION

RUN apk --no-cache add wget unzip && \
  wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_${TARGETARCH}.zip && \
  unzip terraform_${TERRAFORM_VERSION}_linux_${TARGETARCH}.zip && \
  wget https://github.com/terraform-linters/tflint/releases/download/v${TFLINT_VERSION}/tflint_linux_${TARGETARCH}.zip && \
  unzip tflint_linux_${TARGETARCH}.zip && \
  wget https://github.com/gruntwork-io/terragrunt/releases/download/v${TERRAGRUNT_VERSION}/terragrunt_linux_${TARGETARCH} -O terragrunt


FROM python:3.11-slim-buster

LABEL org.opencontainers.image.authors="sre@delogic.io" \
      org.opencontainers.image.description="Docker image for CD/CD pre-commit checks using Terraform/Terragrunt" \
      org.opencontainers.image.licenses="Apache-2.0" \
      org.opencontainers.image.ref.name="debian:buster-slim" \
      org.opencontainers.image.source="https://github.com/delogic-io/containers" \
      org.opencontainers.image.title="pre-commit-tf" \
      org.opencontainers.image.vendor="Delogic.io"

ARG PRECOMMIT_VERSION
ENV DEBIAN_FRONTEND=noninteractive \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY --from=apps /terraform /usr/bin/terraform
COPY --from=apps /tflint /usr/bin/tflint
COPY --from=apps /terragrunt /usr/bin/terragrunt

RUN apt-get update && apt-get install -y --no-install-recommends git \
  && rm -rf /var/lib/apt/lists/* \
  && pip install --no-cache-dir pre-commit==${PRECOMMIT_VERSION}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN APPS=( \
      "terraform" \
      "tflint" \
      "terragrunt" \
    ) && \
    for i in "${APPS[@]}"; do chmod +x /usr/bin/${i}; done

WORKDIR /workdir

CMD ["/bin/bash"]
